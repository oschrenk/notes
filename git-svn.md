# Git + SVN #

## Configuration ##

If you still have to work with some svn repositories you might consider setting up a global file for the svn users so that you don't have to do that for each project.

See [Migrate SVN](#migrate-svn) for some more information, but run add `--global` to add it to the global `.gitconfig` like so:

    $ git config --global svn.authorsfile ~/Desktop/users.txt

## Usecases ##

### Keeping repos in sync ###

	git svn fetch
	git rebase trunk

### Import SVN to local Git ###

Create an empty directory and prepare the repository

    $ mkdir project_tmp
    $ cd project_tmp
    $ git svn init http://code.yoursite.net/project/trunk/ --no-metadata

The `--no-metadata` is for one-shot imports only. Do not use it when you plan to fetch from the repository again.

You get an answer like

    Initialized empty Git repository in /absolute/path/project_tmp/.git/

Now create a file with a list of your svn users and map them to git users, save it to `~/username/users.txt`. The file contents have to look similar to this:

    joesample = Joe Sample <joe.sample@domain.net>
    janedoe = Jane Doe <jane.doe@company.com>

then run

    $ git config svn.authorsfile /path/to/svn.authorsfile

Now we have to get the data from the svn repository by calling

    $ git svn fetch

Depending on the size of your repository and the number of changes you made the process made take while. If everything went fine you get a bunch of messages that look something like this:

    [...]
    r126 = 5ee70c02797f9f4d31238fcdbbcaaf830daf490d (refs/remotes/git-svn)
    	M	doc/notes.textile
    	D	src/com/project/keywords.properties
    	A	src/com/project/Entry.java
    r127 = 4b38f4b2a740e8afc8a134f4d5425dbb2e42dc40 (refs/remotes/git-svn)
    Checked out HEAD:
    http://code.yoursite.net/project/trunk/ r127

You can use `git log` to check if the users were setup correctly. The last step is too to another clone of the repository to get rid of all the stuff that is/was only needed for git svn to work.

    $ cd ..
    $ git clone project_tmp project

	# Creating autmoatic svn to github mirror #

I was interested in mirroring an Open Source project from a SVN repository to a Github repository.

Basically there are two methods:

1. Using git's own `git svn`, which can [handles one path](http://www.fnokd.com/2008/08/20/mirroring-svn-repository-to-github/) e.g.. only the trunk or
2. Using [`svn2git`](https://github.com/nirvdrum/svn2git) which is also able to mirror branches and tags

I'm using a [Linode](http://www.linode.com/) slice to do the mirroring jobs via crontab.

## Push existing Git repository to SVN ##

	1. cd /path/to/git/localrepo
	2. svn mkdir --parents protocol:///path/to/repo/PROJECT/trunk -m "Importing git repo"
	3. git svn init protocol:///path/to/repo/PROJECT -s
	4. git svn fetch
	5. git rebase trunk
	5.1.  git status
	5.2.  git add (conflicted-files)
	5.3.  git rebase --continue
	5.4.  (repeat 5.1.)
	6. git svn dcommit

After `#3` you'll get a cryptic message like this:

	Using higher level of URL: protocol:///path/to/repo/PROJECT => protocol:///path/to/repo

Just ignore that.

When you run `#5`, you might get conflicts. Resolve these by adding files with state `unmerged` and resuming rebase. Eventually, you'll be done; Then sync back to the svn-repo, using `dcommit`.

[source](http://stackoverflow.com/questions/661018/pushing-an-existing-git-repository-to-svn)

### Mirror SVN to Git repositories, With `git svn` ###

I use Osmosis as an example. I only wanted the mirror the trunk from the [SVN Repository](http://svn.openstreetmap.org/applications/utils/osmosis/trunk/).

#### Local ####

	mkdir osmosis
	cd osmosis
	git svn init http://svn.openstreetmap.org/applications/utils/osmosis/trunk
	git svn fetch
	git gc
	hub create
	# repo needs a proper master
	git push origin master
	# create copy of master
	git checkout -b vendor
	git push origin vendor

Preparing it for git flow

	git checkout -b develop
	git flow init -d
	git push origin develop

I then build the project, and added a `.gitignore` file to ignore the auto generated files.

Prepare feature

	git flow feature start ignore

Finish the feature

	git flow feature finish ignore
	git push origin develop

#### Push repository to remote server ####

Time to make the auto-merging happen. First we copy the created git repo to the remote server.

	cd ..
	tar -pczf osmosis.tar.gz osmosis
	scp osmosis.tar.gz user@linode:/home/user

I created a SSH alias for my Linode slice

	ssh user@linode
	tar xvfz osmosis.tar.gz

Create a ssh key for github (don't use a passphrase)

	ssh keygen

Copy the key to your [Github Account settings](https://github.com/account).

#### Create mirroring script ####

	touch svninit2github
	vi svninit2github

	#!/bin/sh
	cd /home/user/github-svn-mirrors/$1
	git svn rebase
	git push origin vendor

Make the script executable

	chmod u+x svninit2github

Try it out for the first time. It should ask you to add `github.com` to the list of known hosts.

#### Create cron job ####

	crontab -e
	00 3 * * * /home/user/scripts/svn2github osmosis

### Mirror SVN to Git repositories, With `svn2git` ###

! svn2git can only push to `master` when rebasing

You need

	sudo apt-get install git-core git-svn ruby rubygems
	sudo gem install svn2git --source http://gemcutter.org

You may have problems running them `gem install` command if you just installed it. Reinitialize your profile.

I will use the Lejos project as an example

Lejos has only a trunk and tags at the root level of the repo, but no branches, so

	mkdir lejos
	cd lejos
	svn2git https://lejos.svn.sourceforge.net/svnroot/lejos --nobranches

Get a coffee or tea, or two or three (it took two hours for me)

	git gc
	hub create
	# repo needs a proper master
	git push origin master
	# push the tags
	git push --tags

#### Push repository to remote server ####

	cd ..
	tar -pczf lejos.tar.gz lejos
	scp lejos.tar.gz user@linode:/home/user

I created a SSH alias for my Linode slice

	ssh user@linode
	tar xvfz lejos.tar.gz

Create a ssh key for github (don't use a passphrase)

	ssh keygen

Copy the key to your [Github Account settings](https://github.com/account).

#### Create mirroring script ####

	touch svn2git2github
	vi svn2git2github

	#!/bin/sh
	cd /home/user/github-svn-mirrors/$1
	svn2git --rebase
	git push origin master
	git push --tags

Make the script executable

	chmod u+x svn2git2github

#### Create cron job ####

Every morning at 5

	crontab -e
	00 5 * * * /home/user/scripts/svn2github lejos