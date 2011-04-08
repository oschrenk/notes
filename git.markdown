# Git #

This document is provided under the terms of the Creative Commons Attribution-Share Alike 3.0 United States License, which may be viewed at the following URL: <http://creativecommons.org/licenses/by-sa/3.0/us/>

In brief, you may use the contents of this document for any purpose,personal, commercial or otherwise, so long as attribution to the author is maintained. Likewise, the document may be modified, and derivative works and translations made available, so long as such modifications and derivations are offered to the public on equal terms as the original document.

## Concept ##

These terms (in parts taken from <sup class="footnote">[1](#fn1)</sup>) should help understanding theconcept behind Git

working tree
:	A **working tree** is any directory on your filesystem which has a repository associated with it (typically indicated by the presence of a sub-directory within it named `.git`.). It includes all the files and sub-directories in that directory.

repository
:	A **repository** is a collection of _commits_, each of which is an archive of what the project's working tree looked like at a past date, whether on your machine or someone else's. It also defines _HEAD_ (see below), which identifies the _branch_ or commit the current working tree stemmed from. Lastly, it contains a set of branches and _tags_, to identify certain commits by name.

the index
:	Also known as _staging area_ or just _stage_.Git does not commit changes directly from the working tree into the repository. Instead, changes are first registered in something called the index. Think of it as a way of “confirming” your changes, one by one, before doing a commit (which records all your approved changes at once).

commit
:	A **commit** is a snapshot of your working tree at some point in time. The state of _HEAD_ (see below) at the time your commit is made becomes that commit’s parent. This is what creates the notion of a "revision history".

branch
:	A **branch** is just a name for a commit, also called a reference. It’s the parentage of a commit which defines its history, and thus the typical notion of a “branch of development”.

tag
:	A **tag** is also a name for a commit, similar to a branch, except that it always names the same commit, and can have its own description text.

master
:	The mainline of development in most repositories is done on a branch called **master**. Although this is a typical default, it is in no way special.

HEAD
:	**HEAD** is used by your repository to define what is currently checked out. If you checkout a branch, HEAD symbolically refers to that branch, indicating that the branch name should be updated after the next commit operation. If you checkout a specific commit, HEAD refers to that commit only. This is referred to as a _detached_ HEAD, and occurs, for example, if you check out a tag name.


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

### Start a new project with Git ###

     $ mkdir notes
     $ cd notes
     $ git init
     $ touch README
     $ git add README
     $ git commit -m 'first commit'

### Clone existing repository ###

    $ git clone git@github.com:username/project.git

### Adding ###

To track a file or directory

    $ git add <path>

To untrack a file

    git rm --cached <path>

#### Undo Adding ####

You might also call it revert adding a file:

    git reset HEAD filename

### Undo changes ###

#### Undo all changes in working dir, deleting untracked files ####

    git reset --hard
    git clean -f -d

You can also use `git clean -f -x -d`, which also deletes the ignored files

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

#### Push to remote branch ####

	git push origin branch

#### Deleting remote branches ####

Kinda weird syntax

	git push origin :branch

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

Using `--index-filter` with git-rm yields a significantly faster version. Like with using `rm` filename, `git rm --cached filename` will fail if the file is absent from the tree of a commit. If you want to "completely forget" a file, it does not matter when it entered history, so we also add `--ignore-unmatch`

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

## Use cases ##

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

### error: failed to push some refs to 'git@github.com:usernname/project.git ###

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

## Sources ##

<p class="footnote" id="fn1"><sup>1</sup> <a href="http://ftp.newartisans.com/pub/git.from.bottom.up.pdf">Git from the bottom up</a> (John Wiegley, 09-12-02)</p>