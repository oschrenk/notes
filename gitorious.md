# Gitorious #

## Installation ##

### Virtual Machine ###

Download Ubuntu Server 11.10 64bit

	wget -O ubuntu-server-11.10-64bit.iso http://www.ubuntu.com/start-download?distro=server&bits=64&release=latest

Create Image

	VBoxManage createvm --name "gitorious" --ostype <ostype> --register
	VBoxManage modifyvm "gitorious" --memory 512 --acpi on --boot1 dvd --nic1 nat
	VBoxManage createhd --filename "<filename>.vdi" --size 5120
	VBoxManage storagectl "gitorious" --name "IDE Controller" --add ide --controller PIIX4
	VBoxManage storageattach "gitorious" --storagectl "IDE Controller" \
	      --port 0 --device 0 --type hdd --medium "gitorious.vdi"
	VBoxManage storageattach "gitorious" --storagectl "IDE Controller"
	      --port 0 --device 1 --type dvddrive --medium ubuntu-server-11.10-64bit.iso
	VBoxHeadless --startvm "gitorious"

Install Ubuntu Server without preconfiguration on Virtual Box

Configure port forwarding to ssh into the virtual box

	VBoxManage setextradata "gitorious" "VBoxInternal/Devices/e1000/0/LUN#0/Config/ssh/Protocol" TCP
	VBoxManage setextradata "gitorious" "VBoxInternal/Devices/e1000/0/LUN#0/Config/ssh/GuestPort" 22
	VBoxManage setextradata "gitorious" "VBoxInternal/Devices/e1000/0/LUN#0/Config/ssh/HostPort" 2222

Configure port forwarding for http

	VBoxManage setextradata "gitorious" "VBoxInternal/Devices/e1000/0/LUN#0/Config/http/Protocol" TCP
	VBoxManage setextradata "gitorious" "VBoxInternal/Devices/e1000/0/LUN#0/Config/http/GuestPort" 80
	VBoxManage setextradata "gitorious" "VBoxInternal/Devices/e1000/0/LUN#0/Config/http/HostPort" 8880
	
Start the remote machine

Install ssh

	sudo install aptitude install openssh-server

Try it out

	ssh localhost

Configure SSH

	mkdir .ssh
	touch .ssh/authorized_keys

Copy your public ssh key from your local machine to the remote machine

	cat ~/.ssh/id_rsa.pub | ssh account@remoteserver.com "cat - >> .ssh/authorized_keys"

Shutdown remote machine

	sudo shutdown -h now

Start guest machine headless

	VBoxHeadless --startvm "gitorious" --vrde=off

ssh into remote machine

	ssh user@localhost -p 2222

### Dependencies ###

Better run the following commands all as su

	sudo su

Install dependencies

	aptitude install \
	    build-essential zlib1g-dev tcl-dev libexpat-dev libxslt1-dev \
	    libcurl4-openssl-dev postfix apache2 mysql-server mysql-client \
	    apg geoip-bin libgeoip1 libgeoip-dev sqlite3 libsqlite3-dev \
	    imagemagick libpcre3 libpcre3-dev zlib1g zlib1g-dev libyaml-dev \
	    libmysqlclient15-dev apache2-dev libonig-dev ruby-dev rubygems \
	    libopenssl-ruby libdbd-mysql-ruby libmysql-ruby \
	    libmagick++-dev zip unzip memcached git-core git-svn git-doc \
	    git-cvs irb

- `mysql-server` will ask you for the password of MySQL's `root` user. Choose one and write it down you need it.
- `postfix` will ask you how to sent mails. I chose "Internet Site" (for sending via SMTP). The option has to be evaluated.
- `postfix` will ask you how to set a system mail name. I chose `gitorious`

Installing gems

	gem install --no-ri --no-rdoc -v 0.9.2.2 rake && \
	    gem install --no-ri --no-rdoc -v 1.1.4 daemons && \
	    gem install -b --no-ri --no-rdoc \
	        rmagick stompserver passenger bundler

Installing the Sphinx Search Server

	wget http://sphinxsearch.com/files/sphinx-0.9.9.tar.gz && \
	    tar -xzf sphinx-0.9.9.tar.gz && \
	    cd sphinx-0.9.9 && \
	    ./configure --prefix=/usr && \
	    make all install
	
Getting Gitorious

	git clone git://gitorious.org/gitorious/mainline.git /var/www/gitorious && \
	    cd /var/www/gitorious && \
	    git submodule init && \
	    git submodule update

Setting path

	ln -s /var/www/gitorious/script/gitorious /usr/bin

Preparing background services

	cd /var/www/gitorious/doc/templates/ubuntu/ && \
	    cp git-daemon git-poller git-ultrasphinx stomp /etc/init.d/ && \
	    cd /etc/init.d/ && \
	    chmod 755 git-daemon git-poller git-ultrasphinx stomp

Enabling background services

	update-rc.d git-daemon defaults && \
		update-rc.d git-poller defaults && \
		update-rc.d git-ultrasphinx defaults && \
		update-rc.d stomp defaults
	
Create symlink as scripts point to `/opt/ruby-enterprise` for `RUBY_HOME`

	ln -s /usr/ /opt/ruby-enterprise

### Apache Configuration ###

Passenger

	$(gem contents passenger | grep passenger-install-apache2-module)

The installation script will guide you through the process. Near the end it will tell you what you'll need to add to your apache configuration. The part you need to copy looks like this:

	Please edit your Apache configuration file, and add these lines:

	    LoadModule passenger_module /var/lib/gems/1.8/gems/passenger-3.0.9/ext/apache2/mod_passenger.so
	    PassengerRoot /var/lib/gems/1.8/gems/passenger-3.0.9
	    PassengerRuby /usr/bin/ruby1.8

	After you restart Apache, you are ready to deploy any number of Ruby on Rails
	applications on Apache, without any further Ruby on Rails-specific
	configuration!

These three lines need to be inserted into

	/etc/apache2/mods-available/passenger.load

I did it like this

	echo "LoadModule passenger_module /var/lib/gems/1.8/gems/passenger-3.0.9/ext/apache2/mod_passenger.so" > /etc/apache2/mods-available/passenger.load
	echo "PassengerRoot /var/lib/gems/1.8/gems/passenger-3.0.9" >> /etc/apache2/mods-available/passenger.load
	echo "PassengerRuby /usr/bin/ruby1.8" >> /etc/apache2/mods-available/passenger.load

Enabling necessary modules

	a2enmod passenger && \
	a2enmod rewrite && \
	a2enmod ssl

Creating the Gitorious Apache2 site

	touch /etc/apache2/sites-available/gitorious

Insert

	<VirtualHost *:80>
		ServerName your.server.com
		ServerAlias localhost
		DocumentRoot /var/www/gitorious/public
	</VirtualHost>

Creating the Gitorious SSL Apache2 site

	touch /etc/apache2/sites-available/gitorious-ssl 

Insert

	<IfModule mod_ssl.c>
		<VirtualHost _default_:443>
			DocumentRoot /var/www/gitorious/public
			SSLEngine on
			SSLCertificateFile    /etc/ssl/certs/ssl-cert-snakeoil.pem
			SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
			BrowserMatch ".*MSIE.*" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
			</VirtualHost>
	</IfModule>

Disable default sites

	a2dissite default && \
	a2dissite default-ssl

Enable Gitorious Sites

	a2ensite gitorious &&
	a2ensite gitorious-ssl

### Database ###

Create MySQL User (select a password before issuing a command)

	mysql -u root -p
	Enter password: (your mysql root password you selected while installing the packages)
	mysql> GRANT ALL PRIVILEGES ON *.* TO 'gitorious'@'localhost' IDENTIFIED BY '<insert password>' WITH GRANT OPTION;
	mysql> FLUSH PRIVILEGES;
	m<sql> EXIT;
	
### Gitorious ###

Create user under which Gitorious will run and serve the Git repositories

	adduser --system --home /var/www/gitorious/ --no-create-home --group --shell /bin/bash git && \
	chown -R git:git /var/www/gitorious

Switch to `git` user

	su - git

Create some directories and files

	mkdir .ssh && \
	touch .ssh/authorized_keys && \
	chmod 700 .ssh && \
	chmod 600 .ssh/authorized_keys && \
	mkdir tmp/pids && \
	mkdir repositories && \
	mkdir tarballs

Exit to `root` user

	exit
	
Creating the Gitorious configuration

	cp config/database.sample.yml config/database.yml && \
	cp config/gitorious.sample.yml config/gitorious.yml && \
	cp config/broker.yml.example config/broker.yml
	
Edit `config/database.yml` and set username and password (edit the right section (not test:, but production:))

	vi config/database.yml
	
Edit `config/gitorious.yml` (edit the right section (not test:, but production:))

	vi config/gitorious.yml

Change the following

- `gitorious_client_port` should be `80`
- `gitorious_host` needs to be the exact hostname that clients will use (cookies get messed up otherwise)
- `repository_base_path` should be `/var/www/gitorious/repositories`
- `archive_cache_dir` should be `/var/www/gitorious/tarballs`
- `archive_work_dir` should be something like `/tmp/tarballs-work`
- `cookie_secret` needs to be set to a random string >= 30 characters (you can use something like `apg -m 64`)

Set the following *tweaks*

- `hide_http_clone_urls` should be `true` (they require extra unknown setup to work)
- `is_gitorious_dot_org` should be `false`

For our virtual environment ssl access makes problems

	use.ssl: false

Switch to git user

	su - git
	
Change directory

	cd /var/www/gitorious

Make sure all gems are in the correct version

	cd /var/www/gitorious/ && \
	bundle install && \
	bundle pack

Lets rake

	export RAILS_ENV=production && \
	bundle exec rake db:create && \
	bundle exec rake db:migrate && \
	bundle exec rake ultrasphinx:bootstrap

Create the Sphinx Cronjob

	crontab -e
	
Add
	
	* * * * * cd /var/www/gitorious && /usr/bin/bundle exec rake ultrasphinx:index RAILS_ENV=production

Create an admin user

	env RAILS_ENV=production && \
	ruby1.8 script/create_admin
	
Return to normal user

	exit
	exit
	
Reboot

	sudo reboot

## Resources ##

- [Lucas Jenß, Installation Guide for Ubuntu 11.04](http://coding-journal.com/installing-gitorious-on-ubuntu-11-04/)
http://blog.kyodium.net/2011/09/install-gitorious-on-ubuntu-1104.html
https://github.com/crazycode/gitorious-ubuntu-sprinkle

postfix: http://www.cpuug.org/index.php?topic=231.0


