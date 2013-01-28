# bzr2git #

Install the fast import plugin

	mkdir -p ~/.bazaar/plugins
	cd ~/.bazaar/plugins
	bzr branch lp:bzr-fastimport fastimport

Install fast import python module dependency

	bzr branch lp:python-fastimport python_fastimport
	cd python-fastimport/
	python setup.py install

Create the git repository in another directory

	mkdir ~/project.git
	cd ~/project.git
	git init
	bzr fast-export --plain ~/path/to/bzr/branch | git fast-import
	git checkout master
