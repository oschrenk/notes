# Apache #

## Installation ##

### Ubuntu ###

	sudo apt-get install apache2

### OSX ###

Comes with the OS

## Default Configuration ##

### OSX ###

	ServerRoot              ::      /usr
	Primary Config Fle      ::      /etc/apache2/httpd.conf
	DocumentRoot            ::      /Library/WebServer/Documents
	ErrorLog                ::      /var/log/apache2/error_log
	AccessLog               ::      /var/log/apache2/access_log
	cgi-bin                 ::      /Library/WebServer/CGI-Executables (empty by default)
	binary                  ::      /usr/sbin/httpd
	start/stop              ::      /usr/sbin/apachectl (start|stop|restart|fullstatus|status|graceful|graceful-stop|configtest|help)

### Ubuntu, OpenSuse ###

	ServerRoot "/etc/apache2"
	ErrorLog /var/log/apache2/error.log
	# Include module configuration:
	Include /etc/apache2/mods-enabled/*.load
	Include /etc/apache2/mods-enabled/*.conf
	# Include all the user configurations:
	Include /etc/apache2/httpd.conf
	# Include the virtual host configurations:
	Include /etc/apache2/sites-enabled/

## Usage ##

### Local Web Development ###

To enable access to your local websites via `http://project` you first have to include the

Edit `/etc/apache2/httpd.conf` and uncomment

	# Virtual hosts
	Include /private/etc/apache2/extra/httpd-vhosts.conf

Restart apache with

	sudo apachectl restart

Now open  `/etc/apache2/extra/httpd-vhosts.conf`. Remove the example virtual hosts and for each project add

	<VirtualHost project1>
		ServerName project1
	    DocumentRoot "/Users/jdoe/development/www/project1"
	</VirtualHost>

The id `project1` is used in the `VirtualHost` directive and as the `ServerName` property

Edit `etc/hosts`

	127.0.0.1	project1

Try accessing your project via `http://project1`.

If you get an `Access Denied error` you have to enable access to that directory. I suggest that all your web development projects have a common top directory that you than can give access to. Edit `/etc/apache2/extra/httpd-vhosts.conf` and add

	<Directory /Users/jdoe/development/www/>
	  Order Deny,Allow
	  Allow from all
	</Directory>

### Which version am I running ###

	httpd -v

## FAQ/Problems ##

### Enable PHP Support ###

Uncomment

	#LoadModule php5_module        libexec/apache2/libphp5.so

in `/etc/apache2/httpd.conf` and restart

### `/usr/sbin/apachectl: line 82: ulimit: open files: cannot modify limit: Invalid argument` ###

The upgrade to OSX 10.6.5 broke `apachectl`

Change line 64 of `/usr/sbin/apachectl` from

	ULIMIT_MAX_FILES="ulimit -S -n `ulimit -H -n`"

to

	ULIMIT_MAX_FILES=""

### `CFURLWriteDataAndPropertiesToResource(/System/Library/LaunchDaemons/org.apache.httpd.plist) failed: -10` ###

You somehow called (or something similar)

    $ apachectl stop

expecting it will use the macports installation and run `/opt/local/apache2/bin/apachectl` . Unfortunately the Apache installation of OS X is first in the `PATH`. To control the pre-installed Apache its better to use `System Preferences > Sharing > Web-Sharing`.
