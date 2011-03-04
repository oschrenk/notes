# Linux #

## Update ##

	apt-get update
	apt-get upgrade
	
To search packages

	apt-cache --name-only search apache

	
## User Managment ##

You shouldn't be working with a `root` account. Create another user first:

Add a user

	adduser --home /home/jdoe \
	        --shell /bin/bash \
	        --ingroup preexisting_group \
			jdoe
	passwd jdoe
	
The default configuration setting `NAME_REGEX="^[a-z][-a-z0-9]*\$"` rules which names aren't allowed and which aren't. You should follow the rule.

Just leave out the `ingroup` parameter for not adding the user to other groups.
		
Remove a user
	
	userdel -r  jdoe      ## Remove user incl. home directory

Allow him to sudo via

	visudo
	
Opens `nano` in my case, below

	# User privilege specification
	root    ALL=(ALL) ALL
	
add

	jdoe ALL=(ALL) ALL

Save the via by exiting via `^X` and save it to the suggested filename. `visudo` takres care of sanity checking.

Open another shell and ssh into your linode with the newly created user. Check if the home directory exists and try if you can `sudo`.

## Security ##

- keeps your system up to date
- user strong password

### Lock down SSH ###

Edit the `/etc/ssh/sshd_config` and change the following settings

	PermitRootLogin no
	PasswordAuthentication no

- you shouldn't access your server as root
- you should use ssh keys to access your system

If you don't have an SSH key, you can generate one via

	ssh-keygen
	
By default your private key will be stored in `~/.ssh/id_rsa` and he private key into `~/.ssh/id_rsa.pub`.

You then want your key to be authorized. You do that by adding your key to the `~/.ssh/authorized_keys` file on the machine you want to log into.

	cat ~/.ssh/id_rsa.pub | ssh account@remoteserver.com "cat - >> .ssh/authorized_keys"

## My Linux ##
	
### Ubuntu ###

	## basics
	apt-get update
	apt-get upgrade
	apt-get install make
	apt-get install cron

	## convenience
	apt-get install ack-grep
	apt-get install bash-completion
	
	## install scms
	apt-get install subversion
	apt-get install git
	apt-get install git-svn
	# apt-get install gitolite
	# hub
	curl http://chriswanstrath.com/hub/standalone -sLo ~/bin/hub && chmod 755 ~/bin/hub
	# git flow	
	wget --no-check-certificate -q -O - https://github.com/nvie/gitflow/raw/develop/contrib/gitflow-installer.sh | sudo sh

	## install node.js
	sudo apt-get install python-software-properties
	sudo add-apt-repository ppa:jerome-etienne/neoip
	sudo apt-get update
	sudo apt-get install nodejs
	curl http://npmjs.org/install.sh | sh

	## java
	sudo apt-get install maven2

	## ruby
	sudo apt-get install ruby
	sudo apt-get install rubygems
	
	## http
	sudo apt-get install curl

### Open Suse ###

	## basics
	apt-get update
	apt-get upgrade
	sudo zypper install make
	sudo zypper install cron

	## convenience
	curl http://betterthangrep.com/ack-standalone > ~/bin/ack && chmod 0755 !#:3
	sudo zypper install bash-completion
	
	## install scms
	apt-get install subversion
	apt-get install git
	apt-get install git-svn
	# apt-get install gitolite
	# hub
	curl http://chriswanstrath.com/hub/standalone -sLo ~/bin/hub && chmod 755 ~/bin/hub
	# git flow	
	wget --no-check-certificate -q -O - https://github.com/nvie/gitflow/raw/develop/contrib/gitflow-installer.sh | sudo sh

	## install node.js
	sudo zypper ar http://download.opensuse.org/repositories/home:/SannisDev/openSUSE_11.3/ SannisDevBuildService 
	sudo zypper in nodejs nodejs-devel


	## java
	sudo apt-get install maven2

	## ruby
	sudo apt-get install ruby
	sudo apt-get install rubygems
	
	## http
	sudo apt-get install curl
	
	

