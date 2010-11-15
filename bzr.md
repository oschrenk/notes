# Bazaar #

## Working with Bazaar

### Information ###

**Working tree status**

	bzr status
	
**Revision log** 

	bzr log
	bzr log foo.py
	
### Merging ###

	bzr merge

## Plugins ##

** List plugins **

	bzr plugins
	
Per user plugins should be installed to `$HOME/.bazaar/plugins/`

	mkdir -p $HOME/.bazaar/plugins/
	cd $HOME/.bazaar/plugins/
	bzr branch lp:bzr-xxx-yyy xxx_yyy
	
For example

	bzr branch lp:bzr-fastimport fastimport
	


