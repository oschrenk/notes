# Apache #

## Default Paths on OSX ##

	ServerRoot              ::      /usr
	Primary Config Fle      ::      /etc/apache2/httpd.conf
	DocumentRoot            ::      /Library/WebServer/Documents
	ErrorLog                ::      /var/log/apache2/error_log
	AccessLog               ::      /var/log/apache2/access_log
	cgi-bin                 ::      /Library/WebServer/CGI-Executables (empty by default)
	binary                  ::      /usr/sbin/httpd
	start/stop              ::      /usr/sbin/apachectl (start|stop|restart|fullstatus|status|graceful|graceful-stop|configtest|help)

## FAQ/Problems ##

### `/usr/sbin/apachectl: line 82: ulimit: open files: cannot modify limit: Invalid argument` ###

The upgrade to OSX 10.6.5 broke `apachectl`

Change line 64 of `/usr/sbin/apachectl` from

	ULIMIT_MAX_FILES="ulimit -S -n `ulimit -H -n`"
	
to

	ULIMIT_MAX_FILES=""

### `CFURLWriteDataAndPropertiesToResource(/System/Library/LaunchDaemons/org.apache.httpd.plist) failed: -10` ###

You somehow called (or something similar)

    $ apachectl stop

expecting it will use the macports installation and run `/opt/local/apache2/bin/apachectl` . Unfortunately the Apache installation of OS X is first in the `PATH`. To control the pre-installed Apache its better to use `System Preferences > Sharing >
Web-Sharing`.
