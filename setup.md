# Setup #

## OSX ##

	## basics
	ruby -e "$(curl -fsSLk https://gist.github.com/raw/323731/install_homebrew.rb)"
	brew install ack
	brew install bash-completion
	
	## scms
	brew install svn
	brew install git
	brew install git-flow
	brew install hub
	
	## node.js
	brew install node
	curl http://npmjs.org/install.sh | sh
	
	## java
	brew install maven
	brew install maven-shell
	brew install play

	## http
	brew install curl
	brew install wget
	brew install httrack

## Ubuntu ##

	## basics
	apt-get update
	apt-get upgrade
	apt-get install make
	apt-get install cron

	## convenience
	apt-get install ack-grep
	apt-get install bash-completion
	
	## edit
	sudo apt-get install gedit-plugins
	
	## install scms
	apt-get install subversion
	apt-get install git
	apt-get install git-svn
	# apt-get install gitolite
	sudo apt-get install meld
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

## Open Suse ##

	## basics
	apt-get update
	apt-get upgrade
	sudo zypper install make
	sudo zypper install cron

	## convenience
	curl http://betterthangrep.com/ack-standalone > ~/bin/ack && chmod 0755 !#:3
	sudo zypper install bash-completion
	
	## java
	# sudo apt-get install maven2
	
	## ruby
	sudo zypper install ruby
	sudo zypper install rubygems
	
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
	curl http://npmjs.org/install.sh | sh
