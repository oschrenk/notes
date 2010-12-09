# Apache #

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
