# PHP & MySQL on OSX

## Apache ##

	sudo port install apache2
	sudo apachectl start
	
Try if it work `http://localhost/`

## PHP ##

	sudo port install php5 +apache2 +pear
	cd /opt/local/apache2/modules
	sudo /opt/local/apache2/bin/apxs -a -e -n "php5" libphp5.so
	sudo port install php5-mysql
	sudo cp /opt/local/etc/php5/php.ini-development /opt/local/etc/php5/php.ini
	
You need to to configure your timezone in `/opt/local/etc/php5/php.ini`

	[Date]
	; Defines the default timezone used by the date functions
	; http://php.net/date.timezone
	;date.timezone =
	
Add index.php to the `dir_module `directive in `httpd.conf`

	<IfModule dir_module>
	    DirectoryIndex index.html index.php
	</IfModule>
	
Add a new MIME type so that Apache will direct files ending in .php to the PHP module for processing. Add the following within the <IfModule mime_module> block. Without this, all you'll see is the text of your PHP scripts

	AddType application/x-httpd-php .php
	AddType application/x-httpd-php-source .phps
	
If you've installed MySQL, tell PHP where it can find the MySQL socket. Set the following in the `php.ini` file:

	mysql.default_socket = /tmp/mysql.sock

Restart the server

	sudo /opt/local/apache2/bin/apachectl graceful
	
### mb string extension ###

	sudo port install php5-mbstring
	sudo /opt/local/apache2/bin/apachectl graceful
	
### PEAR ###

Libraries are installed to 

	/opt/local/lib/php

## MySQL ##

Download the MySQL package for Mac OS X.5 (32 or 64 bits depending on your machine)
Install everything in the package in this order: mysql, the startup item, the preference pane.
Start MySQL in the preference pane.
Test it's working:

	/usr/local/mysql/bin/mysql

### Fix mysql.sock location in php.ini

In `/etc/php.ini`, replace the three occurrences of `/var/mysql/mysql.sock` by `/tmp/mysql.sock`
	pdo_mysql.default_socket=/tmp/mysql.sock
	mysql.default_socket = /tmp/mysql.sock
	mysqli.default_socket = /tmp/mysql.sock

Restart Apache

	sudo apachectl restart
