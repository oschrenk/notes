# Setup #

## OSX ##

	## basics
	ruby -e "$(curl -fsSLk https://gist.github.com/raw/323731/install_homebrew.rb)"
	brew install ack
	brew install bash-completion
	brew install z
	brew install multimarkdown
	
	## system
	# lion broke keyhchain/ssh-agent
	brew install ssh-agent-locker
	mkdir -p ~/Library/LaunchAgents
	cp /usr/local/Cellar/ssh-agent-locker/0.1.0/com.seaandco.geoff.ssh-agent-locker.plist ~/Library/LaunchAgents/
	launchctl load -w ~/Library/LaunchAgents/com.seaandco.geoff.ssh-agent-locker.plist
	# brew install fuse4x
	# brew install sshfs
	
	
	## scms
	brew install svn
	brew install https://raw.github.com/adamv/homebrew-alt/master/other/mercurial.rb
	brew install bazaar
	
	brew install git
	brew install git-extras
	brew install git-flow
	brew install git-sh
	brew install hub
	
	## databases
	brew install mongodb
	brew install redis
	brew install mysql
	brew install postgresql
	brew install sqlite
	
	## geo
	brew install proj
	brew install geos
	brew install osmosis
	brew install postgis
	
	## a/v
	brew install ffmpeg
	brew install flac
	brew install lame
	brew install libmp3splt
	brew install mp3splt
	brew install x264
	brew install xvid
	
	## node.js
	brew install node
	curl http://npmjs.org/install.sh | sh
	
	## node modules
	npm install -g express
	npm install -g markdown-wiki
	npm install -g deja
	npm install -g pegjs
	
	## java
	brew install maven
	brew install maven-shell
	brew install play

	## http
	brew install curl
	brew install wget
	brew install httrack
	
	## dev
	mkdir -p ~/Development
	cd ~/Development
	git clone git@github.com:oschrenk/notes.git
	git clone git@github.com:oschrenk/scripts.git
	
	## home	
	cd ~
	deja clone oschrenk/dotfiles
	
	## update textmate bundles
	. ~/Development/scripts/mateup

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
	sudo apt-get install openjdk-7-jdk
	sudo apt-get install ant
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

