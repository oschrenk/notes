# Git the Basics #

Bart Trojanowski <bart@jukie.net>, July 9th 2008, [repo](git://git.jukie.net/intro-to-git.git/)

## Concepts ##

- Source Control Managment
	- track changes to files
	- repository / database of changes
	- working directory / current state
- Centralized SCM (svn, cvs, ...)
		- server: single database
		- client: working directory & current state
- Decentralized	SCM (git, mercurial, bazaar, ...)
	- anyone can be a server
	- repository coupled with working directory
	- complete history
	- disconnected operation

## Components ##

- working tree (directories and files)
- repository contents
	- files
	- commits (group of changes, metadata)
	- ancestry (parent pointer, not available in SVN)
- directed acyclic graph
- references 
	- tags. normally assigned to a commit)
	- branches. 
	- HEAD. Points to last commit (sometimes detached)
- index or staging area. what is to be commited.

## Operations ##

- Bootstrapping
	- init
	- checkout
	- switch branch
- Modify
	- add, delete, rename
	- commit
- Information
	- status
	- diff
	- log
- Reference
	- tag
	- branch
- Decentralized
	 - clone
	 - pull, fetch
	 - push

In a centralized SCM all operations require the server (single point of failure, bottleneck). In a decentralized anyone can be a server.

## Decentralization ##

Allows for multiple workflows (including centralized)

It is good because of 

- (non-intrusive) micro commits
- detached operation
- no single point of failure
- trivial backups

## Structure ##

- Repository. Metadata. Commits
- Index. Staging array
- Work Tree.

### Repository ###

	.git
	├── HEAD			current checkout reference
	├── config			repo private config
	├── description		repo description (gitweb)
	├── hooks
	│   └── ...			hooking scripts
	├── index			changes to commit
	├── info
	│	├── exclude		repo private 
	│   └── refs		refs
	├── logs
	│   └── ...			reflog data
	├── objects
	│	├── 00			loose objects
	│	├── ..			loose objects
	│	├── ff			loose objects
	│	├── info		
	│	│	└── packs	infos about packs
	│   └── pack
	│		└── ...		packs and indexes
	└── refs
		├── heads
		│  	└── master	master branch
		└── tags
			└── ...		tags

### Objects ###

	type | size | data

- content adressable. SHA-1 acts as unique indentifier of object point to object
- 4 types. 
	- blob. file.
	- tree. directory entry. points to blobs
	- commits. points to trees. who is author/commiter?. points to parent commit
	- tags. similar to a commit. nothing points to it.
- immutable

## Commands ##

	git <options> <command> <options>

There are 100+ commands. This creates fear. But only few are used in every day use (10-20)

	git <command> -h 	# help for each command
	man git 			# manual page for git

### Configuration ###

User configuration

	$HOME/.gitconfig

accesible via `--global` flag. Git needs

	$ git config --global user.name "Your Name"
	$ git config --global user.email "you@domain.tld"

### Bootstrapping ###

	$ mkdir
	$ cd project
	$ git init

### Staging ###

	$ git add file
	$ git add .
	$ git rm file
	$ git mv old new

### Committing ###

Create a commit of all or only staged items
	
	$ git commit -a -m "some comment"

### Inspecting ###

	$ git status

Shows
	
	- staged
	- unstaged
	- untracked

Changes between index and working files

	$ git diff

Changes between HEAD and index

	$ git diff --staged

Changes between HEAD and working files

	$ git diff HEAD

Changes between two commits

	$ git diff $commit $commit

To view infos about commits

	$ git show
	$ git show HEAD
	$ git show HEAD^^^

See commit history. `git log` is awesome!

	$ git log
	$ git log tag..branch
	$ git log HEAD~10..
	$ git log branch1 branch2 ^common
	$ git log -10
	$ git log -10 master@{yesterday}
	$ git log --since="May 1" --until="June 1"
	$ git log --author=fred
	$ git log --commiter=joe
	$ git log --grep="commit.*message.*text"
	$ git log -S "some code change"
	$ git log --pickaxe-regex -S "some.*code.*change"
	$ git log -- some/file
	$ git grep -e "pattern" -- some/file
	$ git grep -e "pattern" branch -- some/file

## Object references ##

- full hash	`6bb1270ffb60cbfef87266d2d4b4abe4218d9c68`
- short hash `6bb127`
- tag `v1.5.6.1`
- local branch `master`
- remote branch `origin/master`
- by message `:/some text`
- checkout `HEAD`
- last fetch `FETCH_HEAD`
- previous head `ORIG_HEAD`
- ...

- `HEAD^ == HEAD~1`  // one commits before
- `HEAD^^^ == HEAD~3` // three commits before
- `@{yesterday} == HEAD@{yesterday}`
- `my-other-branch@{June.1}`
- `master@{3}` // three changes ago

## Branching ##

**References**

- thing that point to a commit
- lightweight
- mutable
- disposable
- 3 basic types

**Local branches**

	$ git branch -l
	branch1
	branch2
	> .git/refs/heads/<branch>

**Local tags**

	$ git tag -l
	tag1
	tag2
	tag3
	> .git/refs/tags/<tag>

**Remote branches**

	$ git branch -r
	fred/master
	joe/master
	joe/anothe-branch
	> .git/refs/remotes/<remote>/<branch>

**Create a branch** "name" on `HEAD` or specified commit

	$ git branch name [commit]

**Switch branches**. Checkout files from "name" branch and optionally force overwriting changed files.

	$ git branch [-f] name

**Create and switch**. Checout files from "name" branch

	$ git checkout -b name [commit]

**Switching with changes**
	
	$ git checkout name
	error: You have local changes to 'filename';
	cannnot switch branches.

## Merging ##

Merge multiple branches

	$ git merge <branch> ...

- creates commit with 2+ parents
- can cause conflicts

## Rebasing ##

Moves new work onto a new baseline

	$ git rebase <branch>

- can cause conflicts

## Remotes ##

Replicate remote repository

	$ git clone <remote>

- local. `/home/git/project.git/`, or `file:///home/git/project.git/`
- http. `http://repo.or.cz/r/git.git`
- native. `git://repo.or.cz/git.git`
- shh. `ssh://bart@jukie.net/~git/project.git/`, or `bart@jukie.net/~git/project.git`

Only update DAG

	$ git fetch

Fast forwards master to `match` `origin/master` and updates the working tree

	$ git merge origin/master

Combine fetch and merge

	$ git pull

Do some work, commit some changes. To publish your work

	$ git push

## Stashing ##

Imagine you hacking away and you have changes you don't want commit yet, but you need to work on something else

	$ git stash "description"

Do your other thing

	git stash apply
