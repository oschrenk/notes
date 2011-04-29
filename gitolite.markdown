# Gitolite #

## Installation ##

	ssh server
	sudo adduser --home /home/gitolite --shell /bin/bash gitolite
	sudo visudo
	## add gitolite as sudo user
	su - gitolite
	mkdir .ssh
	touch .ssh/authorized_keys
	exit
	exit
	
	cat ~/.ssh/id_rsa.pub | ssh gitolite@server.com "cat - >> john.doe.pub"
	
	ssh gitolite@server
	
	git clone git://github.com/sitaramc/gitolite.git
	cd gitolite
	mkdir -p /usr/local/share/gitolite/conf /usr/local/share/gitolite/hooks
	sudo src/gl-system-install /usr/local/bin /usr/local/share/gitolite/conf /usr/local/share/gitolite/hooks
	cd $HOME
	gl-setup john.doe.pub
	
## Adminstration ##

Check out the administration repository

	git clone gitolite@server:gitolite-admin
	
Checkout the installation configuration
	
	cd gitolite-admin
	vi conf/gitolite.conf

	repo    gitolite-admin
	        RW+     =   sena

	repo    testing
	        RW+     =   @all

## FAQ/Problems ##

### fatal: 'gitolite-admin' does not appear to be a git repository ###

	Cloning into gitolite-admin...
	fatal: 'gitolite-admin' does not appear to be a git repository
	fatal: The remote end hung up unexpectedly

The problem is that gitolite appends the authorized keys to the .`ssh/authorized_keys` file. Now if there is a prexisting key without the `command` prefix, it will try to use it, and will fail. Just delete the key without the prefix.