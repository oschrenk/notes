# Case for git #

## Why Git? ##
 
- Index bzw. Stagebereich. 
- Change commits locally. `git c --amend`
- Partial Commits. Keeping changes semantically tied together via `git add --patch`
- it is fast
- merging is much easier
- Stashing code
- Work offline
- Better collaboration
- Tooling (web view, github, bup	)

## Transitioning ##

### git svn ###

Git supports access to SVN repositories via the `git svn` subcommand.

**Checkout**. If you just want to work on the trunk you can use:

	`git svn clone svn://server.network/project/trunk project`

If you further want to track branches and/or tags. 

	`git svn clone svn://server.network/project project --trunk trunk --branches branches --tags tags`

**Fetching (Updating)**. You can pull the lastest changes from the SVN repository via

	git svn fetch

If you have local changes, that may have not been comitted, you may be prompted to stash your changes.

	git stash
	git svn fetch
	git stash apply

**Commiting**. You can comit changes via

	git svn dcommit

## Problem Areas ##

### Maven Release Plugin ###

