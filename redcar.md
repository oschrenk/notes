# Redcar #

## Installation ##

You must have Java installed. You will also need to run these commands for each user on your computer that needs access to Redcar.

	$ sudo gem install redcar
	$ redcar install
	
### Installation from source ###

If you want to contribute to Redcar, you can install it from the source code.

If you're running Windows, as a prerequisite, you'll need to install the rubyzip gem:

	$ gem install rubyzip
	
Download from github, checkout the submodules and install the jars.

	$ git clone git://github.com/redcar/redcar.git
	$ cd redcar
	$ git submodule init
	$ git submodule update
	$ ruby bin/redcar install
	
To run:

	$ ruby bin/redcar
	
### Updating a source build ###

If you are running a source version of Redcar and you have pulled changes from master, then you may have to update your repo:

	$ git submodule update
	$ ruby bin/redcar install