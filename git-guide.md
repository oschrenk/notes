Title: Git Guide 
Author: Oliver Schrenk
Email: Oliver Schrenk <oliver.schrenk@gmail.com>
Copyright: This document is provided under the terms of the Creative Commons Attribution-Share Alike 3.0 United States License, which may be viewed at the following URL: <http://creativecommons.org/licenses/by-sa/3.0/us/>

# Git #

## Why Git ##

- working offline
- Local commits
- Local branches
- staging commits
- easy merging
- rebasing (pull changes from remote on top of local commits)
- rebasing (combine commits)
- speed
- github
- easy accessible version control uris

## Appendix ##

### Definitions ###

These terms (in parts taken from [#Wiegley:2009][]) should help understanding the concept behind Git

working tree
:	A **working tree** is any directory on your filesystem which has a repository associated with it (typically indicated by the presence of a sub-directory within it named `.git`.). It includes all the files and sub-directories in that directory.

repository
:	A **repository** is a collection of _commits_, each of which is an archive of what the project's working tree looked like at a past date, whether on your machine or someone else's. It also defines _HEAD_ (see below), which identifies the _branch_ or commit the current working tree stemmed from. Lastly, it contains a set of branches and _tags_, to identify certain commits by name.

the index
:	Also known as _staging area_ or just _stage_. Git does not commit changes directly from the working tree into the repository. Instead, changes are first registered in something called the index. Think of it as a way of “confirming” your changes, one by one, before doing a commit (which records all your approved changes at once).

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


[#Wiegley:2009]: John Wiegley. [Git from the Bottom Up](http://ftp.newartisans.com/pub/git.from.bottom.up.pdf). 09-12-02