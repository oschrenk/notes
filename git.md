# Git #

## Setup ##

### Installation ###

    $ sudo port install git-core +svn

### Configuring Git ###

    $ git config --global color.ui "auto"
    $ git config --global user.name "FirstName LastName"	
    $ git config --global user.email "your@email.address"

#### Global ignores ####

You don't have to ignore files/paths on a per project basis. You can also have a global ignore list. This is helpful when ignoring such files as `.DS_Store`, which OS X tends to create.

Set the global ignore files

    $ git config --global core.excludesfile ~/.gitignore

Now you can add ignores by calling

    $ echo .DS_Store >> ~/.gitignore

#### Configuring for use with git svn ####

If you still have to work with some svn repositories you might consider setting up a global file for the svn users so that you don't have to do that for each project.

See [Migrate SVN](#migrate-svn) for some more information, but run add `--global` to add it to the global `.gitconfig` like so:

    $ git config --global svn.authorsfile ~/Desktop/users.txt

## Basic Git ##

### Creating a repo ###

Create a repo

     $ mkdir notes
     $ cd notes
     $ git init
     $ touch README
     $ git add README
     $ git commit -m 'first commit'

Or just clone it

    $ git clone git@github.com:username/project.git

### Changes ###

#### Undo all changes in working dir, deleting untracked files ####

    git reset --hard
    git clean -f -d

You can also use `git clean -f -x -d`, which also deletes the ignored files

### Staging ###

To stage a file or directory

    $ git add <path>

#### Unstage a file ####

	git reset HEAD path/to/file

#### Restore deleted file ####

    git checkout HEAD^ foo.bar

### Commit ###

    $ git commit /path/file
    $ git commit -m 'message' /path/file
    $ git commit -a /path/auto-added.file

### Rename ###

    $ git mv <src> <dst>

### Diff ###

    $ git diff

#### Revert ####

Unlike _SVN_ it is **NOT** `svn revert` instead use

    $ git checkout filename

to overwrite changes.

### Branching ###

#### Show Branches ####

Show the available branches (an asterisk `*` represents the active branch)

    $ git branch 

with options

*   `-r` show the remote branches
*   `-a` show all branches

#### Create branches ####

Make a new branch called "experiment"

    $ git branch experiment

#### Deleting branches ####

    $ git branch (-d | -D) <branchname>...

*   `-d` Delete a branch. The branch must be fully merged in HEAD
*   `-D` Delete a branch irrespective of its merged status.

#### Rename branch ####

	git branch -m old_branch new_branch

##### Delete remote branches #####

It has a weird syntax (always a indicator that you should not use it)

	git push origin :newfeature

#### Push to remote branch ####

	git push origin branch

#### Working with branches ####

Switch to a branch called "experiment". Git will warn you if you have uncommitted changes.

    $ git checkout experiment

#### Track remote branches ####

As of Git 1.7.0:

	$ git branch --set-upstream foo upstream/foo

For example

	$ git branch --set-upstream master origin/master

### Merging ###

Just change into the branch you want changes to be merged into and type

    $ git merge <branch-name-to-be-merged-in> 

If there are conflicts, you can take a look at all the versions with

    # common base version of the file
    $ git show :1:file.txt

    # 'our' version of the file
    $ git show :2:file.txt

    # 'their' version of the file
    $git show :3:file.txt

#### Resolve conflict by using remote version verbatim ####

    $ git show :3:file.txt > file.txt
    $ git file.txt

### Tagging ###

To tag a commit (_lightweight tag_)

    $ git tag name-of-tag

But its better to create an _annotated tag_ by supplying `-a` option and adding a message with `-m`

    $ git tag -a name-of-tag <sha1-id> -m "message"

To push your tags to a remote repository, use the following command to push all tags:

    $ git push origin --tags

#### Rename tag ####

Don't do it. Admit you screwed up. Create a tag with the correct name pointing to the same commit an move on. This keeps your team sane.

If you're a lone ranger you don't have a team to keep sane so go ahead.

Create a tag based on old tag

	git tag new_tag old_tag
	
Delete tag locally

	git tag -d old_tag

Delete tag on remote (this hurts other developers and has therefore a ugly syntax)	
	
	git push origin :refs/tags/old_tag 

## Advanced Git ##

### Track/ignore changes ###

Sometimes you make changes to a file, which you don't want to commit. Adding the file to `.gitignore` doesn't work as changes will still be commited if you use the `-a` parameter.

To ignore changes of a file simply:

    git update-index --assume-unchanged <file>

To track changes again:

    git update-index --no-assume-unchanged <file>

### Remove last commit from history ###

    git rebase --onto HEAD~1 HEAD

### Remove a file/directory from history ###

Suppose you want to remove a file (containing confidential information or copyright violation) from all commits:

    $ git filter-branch --tree-filter 'rm filename' HEAD

However, if the file is absent from the tree of some commit, a simple `rm` filename will fail for that tree and commit. Thus you may instead want to use `rm -f` filename as the script.

Using `--index-filter` with `git-rm` yields a significantly faster version. Like with using `rm` filename, `git rm --cached filename` will fail if the file is absent from the tree of a commit. If you want to "completely forget" a file, it does not matter when it entered history, so we also add `--ignore-unmatch`

    $ git filter-branch --index-filter 'git rm --cached --ignore-unmatch filename' merge-point..HEAD

Or if you want to remove a directory from the history you have to call

    $ git filter-branch --index-filter 'git rm -rf --cached --ignore-unmatch filename' merge-point..HEAD

The normal use case would be to call the changes to `HEAD` and not to a range with `merge-point..HEAD`.

While the history is rewritten, the files might still be in your local repository until they have been garbage collected. You might want to [cleanup](#cleanup) your repository.

Make sure you get all files and directories. Maybe you have renamed the file or directory in the past.

### Cleanup and reclaiming space ###

    $ rm -rf .git/refs/original/
    $ git reflog expire --all
    $ git gc --aggressive --prune

### Backup your local repository ###

    # --bare creates a bare repository
    # forces copying of files instead of hard-linking to it
    $ git clone --bare --no-hardlinks /home/proj-a /tmp/proj-a.git

### Splitting a subpath into a new repository ###

First of we clone the project

    $ git clone git://github.com/user/project.git

or make a [local copy](#backup-repository). Finally re-write the history to just contain the files that are in `subdirectory`:

	$ cd project  
	$ git filter-branch --prune-empty --subdirectory-filter name\of\subdirectory master

Then change the remote repository and push the changes.

If you want you can then remove all infos about the subdirectory in your repository (or better on a backup) by [removing the path from history](#remove-file-from-history).

### Changing author in Git ###

    git filter-branch --commit-filter '
    	if [ "$GIT_COMMITTER_NAME" = "<Old Name>" ];
    	then
    	        GIT_COMMITTER_NAME="<New Name>";
    	        GIT_AUTHOR_NAME="<New Name>";
    	        GIT_COMMITTER_EMAIL="<New Email>";
    	        GIT_AUTHOR_EMAIL="<New Email>";
    	        git commit-tree "$@";
    	else
    	        git commit-tree "$@";
    	fi' HEAD

If you want to change

    John Doe <jd@host.tld>

into

    Jane Doe <jd@host.tld>

you would call

    git filter-branch --commit-filter '
    	if [ "$GIT_COMMITTER_NAME" = "John Doe" ];
    	then
    	        GIT_COMMITTER_NAME="Jane Doe";
    	        GIT_AUTHOR_NAME="Jane Doe";
    	        GIT_COMMITTER_EMAIL="jd@host.tld";
    	        GIT_AUTHOR_EMAIL="jd@host.tld";
    	        git commit-tree "$@";
    	else
    	        git commit-tree "$@";
    	fi' HEAD

## FAQ: Problems/Error Messages ##

### Git behind a proxy ###

Taken from [emilsit.net](http://www.emilsit.net/blog/archives/how-to-use-the-git-protocol-through-a-http-connect-proxy/)

> Fortunately, most corporate firewalls allow for tunneling connections through their HTTP proxies, using HTTP CONNECT. This is normally used for allowing browser to connect to secure websites (using SSL over port 443), but if you are lucky, you can have your firewall administrator configure the proxy to also allow CONNECT for port 9418, which is the port used by git.

Install `socat`

    $ sudo port install socat

Create a script called `gitproxy` somewhere in your path

    #!/bin/sh
    # Use socat to proxy git through an HTTP CONNECT firewall.
    # Useful if you are trying to clone git:// from inside a company.
    # Requires that the proxy allows CONNECT to port 9418.
    #
    # Save this file as gitproxy somewhere in your path (e.g., ~/bin) and then run
    #   chmod +x gitproxy
    #   git config --global core.gitproxy gitproxy
    #
    # More details at http://tinyurl.com/8xvpny
     
    `# Configuration. Common proxy ports are 3128, 8123, 8000.
    _proxy=proxy.yourcompany.com
    _proxyport=3128`
     
    `exec socat STDIO PROXY:$_proxy:$1:$2,proxyport=$_proxyport`

Configure git to use it:

    $ git config --global core.gitproxy gitproxy

### `error: src refspec master does not match any.` ###

The error message leads to the conclusion that you do not have a master branch in your local repository. Either push your main development branch (`git push origin my-local-master:master` which will rename it to master on github) or make a commit first. You can not push a completely empty repository.

### Your branch is ahead of &#8216;origin/master' by xx commits. ###

Recently a repository of mine gave me

    # On branch master
    # Your branch is ahead of 'origin/master' by xx commits.

When calling

    $ git status

As its a local repository, only I work on It kinda baffled my as to
why this message occurs. As expected `.git/config` shows

    	[remote "origin"]
    	fetch = +refs/heads/*:refs/remotes/origin/*
    	url = /Users/some/path

My guess is that I accidentally called `git push origin master`. In my
case I can safely delete the remote branch

    $ git remote rm origin

Another case might be:  

When I did a `git push github master` Git reported that `Everything up-to-date`, which I found weird. Of course the problem was easy to solve and easy to spot. I forgot to remove `origin` as remote source when I added `github` as my default remote. Also both pointed to the same repository origin/master was reporting the "problem" . Git obviously stores infos about the stat of remotes.

An easy

    git remote rm origin

solved the problem for me, as I don't need two copies of the same remote.

### error: failed to push some refs to 'git@github.com:username/project.git ###

I tried pushing changes to github and got

    $ git push github master
    To git@github.com:username/project.git
     ! [rejected]        master -> master (non-fast forward)
    error: failed to push some refs to 'git@github.com:username/project.git'
    To prevent you from losing history, non-fast-forward updates were rejected
    Merge the remote changes before pushing again.  See the 'non-fast forward'
    section of 'git push --help' for details.

Very confusing error message. All it means (at least in my case) that you have to pull changes made to the remote repository.

    $ git pull github master
    From github.com:username/project
     * branch            master     -> FETCH	
    Merge made by recursive.
     file.textile |   14 ++++++++++++++
     1 files changed, 14 insertions(+), 0 deletions(-)

### Commited to non-existing branch ###

I recently used submodules and forgot to checkout a branch before I commited

	git commit -m "My excellent commit"
	[detached HEAD d2bdb98] My excellent commit
	3 files changed, 3 insertions(+), 3 deletions(-)
	git push origin master
	Everything up-to-date

and I wondered "No. I just made changes. It isn't up to date"

I found a solution [here](http://edspencer.net/2009/10/git-what-to-do-if-you-commit-to-no-branch.html)

	# checkout the branch you wanted to commit to
	git checkout master
	# merge into commit SHA
	git merge d2bdb98

Then push again

	git push origin master

### fatal: git checkout: updating paths is incompatible with switching branches. ###

I was trying to fetch a remote branch when I got the error message:

	fatal: git checkout: updating paths is incompatible with switching branches.
	Did you intend to checkout 'origin/1.0' which can not be resolved as commit?

This occurs when you are trying to checkout a remote branch that your local git repo is not aware of yet. Try:

	git remote show origin

If the remote branch you want to checkout is under "New remote branches" and not "Tracked remote branches" then you need to fetch them first:

	git fetch
	
Now you can:

	git checkout --track -b 1.0 origin/1.0
	
or shorter

	git co -t origin/1.0