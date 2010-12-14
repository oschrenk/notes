# Davical #

Info taken from official [wiki entry](http://wiki.davical.org/w/Ubuntu/Lucid)

My motivation was trying out the CardDav implementation

## Installation ##

Install requirements

	sudo apt-get install apache2
	sudo apt-get install php5 libapache2-mod-php5
	sudo apt-get install postgresql

To get the newest features

	sudo apt-key advanced --keyserver pgp.net.nz --recv-keys F6E0FA5CF0307507BB23A512EAFCFEBF8FEB8EBF
	echo "deb http://debian.mcmillan.net.nz/debian lenny awm " | sudo tee /etc/apt/sources.list.d/davical.list
	sudo apt-get update 
	sudo apt-get install davical
	
## Configuration ##

### Apache ###

Restart your Apache to make sure it can handle PHP files.

	sudo /etc/init.d/apache2 restart
	
Verifiy your Apache and PHP configuration by

	cd /var/www
	sudo mv index.html index.php
	vi index.php
	
I inserted somewhere

	<?php
	  phpinfo();
	?>

Watch the magic in your browser and remove the info again.

Copy the default virtual host setup

	sudo cp /etc/apache2/sites-available/default /etc/apache2/sites-available/mydavicalsite

Edit the virtualhost config file:

	sudo nano /etc/apache2/sites-available/mydavicalsite
so that these lines were used (instead of the original ones):

	#
	# Virtual Host def for Debian package DAViCal
	<VirtualHost *:80>
	 DocumentRoot /usr/share/davical/htdocs
	 DirectoryIndex index.php index.html
	 ServerName mydavicalsite.dyndns.org
	 ServerAlias calendar.mydavicalsite.dyndns.org
	 Alias /images/ /usr/share/davical/htdocs/images/
	 <Directory /usr/share/davical/htdocs/>
	     AllowOverride None
	     Order allow,deny
	     Allow from all
	 </Directory>
	 php_value include_path /usr/share/awl/inc
	 php_value magic_quotes_gpc 0
	 php_value register_globals 0
	 php_value error_reporting "E_ALL & ~E_NOTICE"
	 php_value default_charset "utf-8"
	</VirtualHost>

Enable the site by symlinking it to the enabled sites

	sudo ln -s /etc/apache2/sites-available/mydavicalsite /etc/apache2/sites-enabled/mydavicalsite

and restart your apache

	sudo /etc/init.d/apache2 restart

### PostgreSQL ###

This is for Ubuntu Lucid (10.04) taken from [here](https://help.ubuntu.com/community/PostgreSQL)

	sudo -u postgres psql postgres

Set a password for the postgres superuser:

	\password postgres

Type Control+D to exit the posgreSQL prompt (You may need to quit using `\q` when you are done).

Create the initial database

	sudo -u postgres createdb mydb

#### Set up DaviCal PostgreSQL users ####

Create the DaviCal users (first becoming the the system root superuser, using sudo su, then becoming the database superuser, postgres):

	sudo su
	su postgres -c "createuser davical_app"
	exit
	
You will get asked about superusers, roles and databases, but just say "No" to all questions. This functional ID needs only minimum rights. Repeat the process to create one more user, `davical_dba`:

	sudo su
	su postgres -c "createuser davical_dba"
	exit
	
Edit the configuration file `pg_hba.conf`:

	sudo nano /etc/postgresql/8.4/main/pg_hba.conf
	
Add the following 4 lines near (or at) the top;

	local all all trust
	local davical davical_dba trust
	local davical davical_app trust
	host davical davical_app 127.0.0.1/32 trust
	
(The last line is for accessing the database over TCP/IP, assuming the database and the apache2 server are on the same computer.)

Restart the postgreSQL server:

	sudo /etc/init.d/postgresql-8.4 restart

#### Setup the DAViCal database ####

Run the database creation/installation script:

	sudo su
	su postgres -c /usr/share/davical/dba/create-database.sh
	exit

You should see some output like this:

	Supported locales updated.
	Updated view: dav_principal.sql applied.
	CalDAV functions updated.
	RRULE functions updated.
	Database permissions updated.
	NOTE
	====
	*  The password for the 'admin' user has been set to 'gQshTv8w'"
	

Write down the admin password when it is displayed. You will need it later. Once the creation script has run correctly, again edit the `pg_hba.conf` file:

	sudo nano /etc/postgresql/8.4/main/pg_hba.conf

and remove the line

	local all all trust

(This step is not strictly necessary for the installation, but do you really want anybody with a local account to have free access to all the databases?)

Restart the database daemon:

	sudo /etc/init.d/postgresql-8.4 restart

#### Test your database ####

	sudo su
	su postgres
	psql davical
	davical=# \z
	davical=# \q
	exit
	exit
	
You should see a table with a list of access permissions to "davical_dba". (Typing "\q" exits pqsl.)

### DAViCal ###

Edit your own configuration file in `/etc/davical`. (Use your own domain name instead of the one in the example, of course.):

	sudo nano /etc/davical/mydavicalsite.dyndns.org-conf.php

You can merely include the following lines:

	<?php
	  $c->admin_email = 'admin@example.net';
	  $c->system_name = "DAViCal CalDAV Server";
	  $c->pg_connect[] = 'dbname=davical port=5432 user=davical_app';
	
## Startup ## 
	
	
## FAQ/Problems ##

### DBI connect('dbname=davical','davical_dba',...) failed: FATAL:  Ident authentication failed ###

During the creation of the initial davical database the database got created but the script somehow wasn't able to connect. I had to redo the steps and had to drop the database 

	sudo -u postgres psql postgres
	DROP DATABASE davical;
	\q

The problem was that I added the lines in the `/etc/postgresql/8.4/main/pg_hba.conf` at the bottom and not above the current configuration

### Starting/Stopping Server ###

	sudo /etc/init.d/apache2 [start|restart|stop] 
	sudo /etc/init.d/postgresql-8.4 [restart|stop]
	
