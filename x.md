# X #

## Uninstall XQuartz ##

	sudo rm -rf /opt/X11* /Library/Launch*/org.macosforge.xquartz.* /Applications/Utilities/XQuartz.app /etc/*paths.d/*XQuartz
	sudo pkgutil --forget org.macosforge.xquartz.pkg

## Error: Can't open display: ##

Check your `DISPLAY` settings.

I had to manually reset it. Don't set a hostname

	export DISPLAY=:0.0

## X11 over SSH ##

	ssh -X user@remote
