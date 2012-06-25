Title: Git Guide 
Author: Oliver Schrenk
Email: Oliver Schrenk <oliver.schrenk@gmail.com>
Copyright: This document is provided under the terms of the Creative Commons Attribution-Share Alike 3.0 United States License, which may be viewed at the following URL: <http://creativecommons.org/licenses/by-sa/3.0/us/>

# Git #

## What is Git ##

TODO

## Why Git ##

### Offline productivity, speed, and multitasking ###

Git encourages multitasking and experimentation. Fast and easy local branching means the ability to keep bugs and features you’re working on in different workspaces, and to experiment more with throwaway branches. Having everything local means you can have fast diffs and history logs, and commit to your repo while on the go without being online.

### Less cognitive noise ###

Working offline and local branching and the staging process helps dampening the *cognitive noise*.

### Remote collaboration and code review ###

The ability to pull down other people’s changesets for code review and collaboration is made easy with git’s multiple remotes capability. Being able to create cheap local branches and experiment with integrating other developer’s changesets makes it ideal for open source projects and outsourced collaboration with junior developers, where you want to review other people’s code before it becomes part of the master branch. And because anyone who has access to a repo can clone it, they can do development on a fork without asking for your approval, which also encourages experimentation.

Even the web interface talking about code, it offers easy accessible version control URIs.

### Changeset cleanliness ### 

How many times have you tried to trace down a feature across many commits, or even worse, tease apart changesets that are clusters of unrelated features? Git offers multiple ways to keep changesets clean, from the index/staging area, to topic branches, to amending commits and completely rewriting your commit history. It’s amazing what clean changesets can do to save you time in code review and release management.

## Why is DVCS better than Subversion ? ##

The claim of why merging is better in a DVCS than in Subversion was largely based on how branching and merge worked in Subversion a while ago. Subversion prior to 1.5.0 didn't store any information about when branches were merged, thus when you wanted to merge you had to specify which range of revisions that had to be merged.

### So why did Subversion merges *suck*? ###

Ponder this example:

	      1   2   4     6     8
	trunk o-->o-->o---->o---->o
	       \
	        \   3     5     7
	b1       +->o---->o---->o

When we want to merge `b1`'s changes into the trunk we'd issue the following command, while standing on a folder that has trunk checked out:

	svn merge -r 3:7 {link to branch b1}

which will attempt to merge the changes from b1 into your local working directory. And then you commit the changes after you resolve any conflicts and tested the result. When you commit the revision tree would look like this:

	      1   2   4     6     8   9
	trunk o-->o-->o---->o---->o-->o      "the merge commit is at r9"
	       \
	        \   3     5     7
	b1       +->o---->o---->o

However this way of specifying ranges of revisions gets quickly out of hand when the version tree grows as subversion didn't have any meta data on when and what revisions got merged together. Ponder on what happens later:

	           12        14
	trunk  …-->o-------->o
	                                     "Okay, so when did we merge last time?"
	              13        15
	b1     …----->o-------->o

This is largely an issue by the repository design that Subversion has, in order to create a branch you need to create a new virtual directory in the repository which will house a copy of the trunk but it doesn't store any information regarding when and what things got merged back in. That will lead to nasty merge conflicts at times. What was even worse is that Subversion used two-way merging by default, which has some crippling limitations in automatic merging when two branch heads are not compared with their common ancestor.

To mitigate this Subversion now stores meta data for branch and merge. That would solve all problems right?

On a centralized system, like subversion, _virtual directories_ suck. Why? Because everyone has access to view them… even the garbage experimental ones.

Branching is good if you want to experiment but you don't want to see everyones' and their aunts experimentation. This is serious **cognitive noise**. The more branches you add, the more crap you'll get to see.
The more public branches you have in a repository the harder it will be to keep track of all the different branches. So the question you'll have is if the branch is still in development or if it is really dead which is hard to tell in any centralized version control system.
Most of the time, from what I've seen, an organization will default to use one big branch anyway. Which is a shame because that in turn will be difficult to keep track of testing and release versions, and whatever else good comes from branching.

### So why are DVCS, such as Git, Mercurial and Bazaar, better than Subversion at branching and merging? ###

There is a very simple reason why: **branching is a first-class concept**. There are no virtual directories by design and branches are hard objects in DVCS which it needs to be such in order to work simply with synchronization of repositories (i.e. *push* and *pull*).

The first thing you do when you work with a DVCS is to clone repositories (git's `clone`, hg's `clone` and bzr's `branch`). Cloning is conceptually the same thing as creating a branch in version control. Some call this forking or branching (although the latter is often also used to refer to co-located branches), but it's just the same thing. Every user runs their own repository which means you have a per-user *branching* going on.

The version structure is not a tree, but rather a graph instead. More specifically a directed acyclic graph (DAG, meaning a graph that doesn't have any cycles). You really don't need to dwell into the specifics of a DAG other than each commit has one or more parent references (which what the commit was based on). So the following graphs will show the arrows between revisions in reverse because of this.

A very simple example of merging would be this; imagine a central repository called `origin` and a user, Alice, cloning the repository to her machine.

	         a…   b…   c…
	origin   o<---o<---o
	                   ^master
	         |
	         | clone
	         v

	         a…   b…   c…
	alice    o<---o<---o
	                   ^master
	                   ^origin/master

What happens during a clone is that every revision is copied to Alice exactly as they were (which is validated by the uniquely identifiable hash-id's), and marks where the origin's branches are at.
Alice then works on her repo, committing in her own repository and decides to push her changes:

	         a…   b…   c…
	origin   o<---o<---o
	                   ^ master

	              "what'll happen after a push?"


	         a…   b…   c…   d…   e…
	alice    o<---o<---o<---o<---o
	                             ^master
	                   ^origin/master

The solution is rather simple, the only thing that the _origin_ repository needs to do is to take in all the new revisions and move it's branch to the newest revision (which git calls "fast-forward"):

	         a…   b…   c…   d…   e…
	origin   o<---o<---o<---o<---o
	                             ^ master

	         a…   b…   c…   d…   e…
	alice    o<---o<---o<---o<---o
	                             ^master
	                             ^origin/master

The use case, which I illustrated above, **doesn't even need to merge anything**.

So the issue really isn't with merging algorithms since three-way merge algorithm is pretty much the same between all version control systems. **The issue is more about structure than anything**.

## Birds Eye View ##

### Graph Theory ###

In order to understand Git we first visit Computer Science 101 and discuss a little Graph Theory.

The city of Königsberg. A city set on both sides of the river Pregel. It included two islands connected to each each other and the mainland by seven bridges.

	               +------+
	  +-----------+|      |
	  |            |      |+------------+
	  | +---------+|      |             |
	  | |          +------+             |
	  | |                               |
	  + +                               +
	+-----+                          +-----+
	|     |                          |     |
	|     |+------------------------+|     |
	|     |                          |     |
	+-----+                          +-----+
	  + +                               +
	  | |                               |
	  | |          +------+             |
	  | +---------+|      |             |
	  |            |      |+------------+
	  +-----------+|      |
	               +------+

Now imagine this: You want to walk through the city and cross every bridge exactly once. Is this possible?

In 1735, a mathematician named Leonhard Euler proved that such a route doesn't exist. In doing so, he laid the groundworks for a new field of mathematics, we now call graph theory.

Euler reduced the problem to this:
- each land mass can be represented by a point
- each bridge is a line connecting two points

Think of graph theory as the study of **places to go, and ways to get there**. Only in graph theory the places are called **nodes** and the ways are called **edges**

To do something with graphs we (developers) normally apply labels to the edges and nodes. Think of a public transit plan. Every station has a name and every line is a train, or an edge connecting these stations. 

By adding labels can not only attach informations like names but also semantic meaning like relationships or directions. Having directions or not is one of the key characteristics of a graph. They are either **directed** or **undirected**. Think of it like a street map, there are two-way streets and one way street.

Having directions in a graph also means that sometimes you can't reach nodes from any node in your graphs. 
		
		 F-->G-->H
		/
	A-->B-->C-->D-->E
			\
			 I-->J-->K

Once you went from `B` to `F`, you can't go back. You can only reach `G` and from there `H`

### Graphs and Git ###

A Git repository is one giant graph.

When you interact with git, you're working with commits in one or the other. A Git commit consists of two things

- a pointer the state of your code
- zero or more pointers to *parent* commits.

**A Git commit is a node in a graph**.

### References ###

When visualizing a Git repository either through graphical tools like GitX or 

	git log --pretty=oneline --abbrev-commit --branches=* --graph --decorate
	
you may notice some labels like `master`. 

These are references. **References are pointers to commits**. There are three kind of references:
- `local branch references` a local branch reference is a file in your projects `.git/refs/heads` directory containing a 40-byte identifier to the commit the reference points to. This is aprt of why branching is "cheap". Creating a branch means writing 40 byte to a file. Local branch references are specific to a single repository: your local one.
- `remote branch references` are also specific to a single repository, but one that's been defined as a remote.
- `tag references` are basically like branch references that never move. **Once you created a tag, it will never change** (unless you `--force` it; which is not a good idea). You can use it to mark specific versions.

We now come to one of the main ideas:

**References make commits reachable**

It is like nailing down a part of the graph that you might want to come back to. Or think of creating a branch is like saving a game, before you fight the boss.

## Transitioning from SVN to GIT ##

The best way to try Git inside the organization is go to Git-SVN mode. It will allow to use SVN as primary repo, but allow Git features like local branching, cool merging, stashes and other stuff described here.

### Install git

- OS X: run `brew install git`
- Windows: [Download](http://code.google.com/p/msysgit/downloads/list?q=full+installer+official+git) and install
- Debian based (e.g. Ubuntu): `apt-get install git-core git-svn`
- Fedora `yum install git-core git-svn`

### Setup your environment

Setup local user information, example

	git config --global user.name "John Doe"
	git config --global user.email "john.doe@domain.com"

The default choice taken by the GIT developers is to use *nix style EOL character (LF only) whenever possible. The recommendation is that locally each user converts to *nix LF endings when committing (in order to have common SHA-1 hashes) and to locally set the recreation option to their local system setting.

If you are on Windows, it is recommended to turn off `autocrlf`

	git config --global core.autocrlf false

If your are on *nix, set

	git config --global core.autocrlf input

### Working with git svn ###

Git supports access to SVN repositories via the `git svn` subcommand.

**Checkout**. If you just want to work on the trunk you can use:

	`git svn clone svn://server.network/project/trunk project`

If you further want to track branches and/or tags. 

	`git svn clone svn://server.network/project project --trunk trunk --branches branches --tags tags`

**Branching**. By default, git repository contains one branch called master. Good style is do not do direct commits into master at all, just keep it as synchronization point between SVN repository and your local repository. So, to do actual job you to create branch.

	git checkout -b bugfix-id-1453

This command will create new branch called `bugfix-id-1453` and immediately switch to it. If you haven't noticed how that happened - please welcome to git's high performance world.

**Changes**. You changed some file and ready to commit. In git it's a 2 steps operation. First, you need to put content to stage.. second, you store the content in new commit object.

    git add .
    git commit -m "issue has been fixed"

You can commit changes back to SVN via

You've fixed a bug and now fixed is placed in `bugfix-id-1453`. But you should give it back to rest of the world.

Checkout your master branch:

	git checkout master

**Rebase**. Get the latest changes from SVN:

	git svn rebase

Now SVN server and local master branch are identical. We have to merge our fix into master:

    git merge bugfix-id-1453

**Commit**. Now master contains previous SVN state + new fix. We have to synchronize local repository and SVN server:

	git svn dcommit

## Appendix ##

### Definitions ###

These terms (in parts taken from [#Wiegley:2009][]) should help understanding the concept behind Git

working tree
:	A **working tree** is any directory on your file system which has a repository associated with it (typically indicated by the presence of a sub-directory within it named `.git`.). It includes all the files and sub-directories in that directory.

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

[#Livingston:2011]: Sam Livingston-Gray. [Think Like (a) Git](http://think-like-a-git.net/). 2011
[#Wiegley:2009]: John Wiegley. [Git from the Bottom Up](http://ftp.newartisans.com/pub/git.from.bottom.up.pdf). 09-12-02
[#Beletsky:2012]: Alexamder Beletsky. [A Year with Git](http://java.dzone.com/articles/year-git). 13-06-2012.
[#Piotrowski:2010]: Mikael Piotrowski. [How and/or why is merging in Git better than in SVN?](http://stackoverflow.com/questions/2471606/how-and-or-why-is-merging-in-git-better-than-in-svn/2472251#2472251). March 2010.
[#Pritzker:2011]: Yan Pritzker [Git Workflows](http://yanpritzker.com/git-book/). 2011.