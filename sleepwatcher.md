# Sleepwatcher #

	brew install sleepwatcher

	For SleepWatcher to work, you will need to read the following:

	  /usr/local/Cellar/sleepwatcher/2.2/ReadMe.rtf

	Ignore information about installing the binary and man page,
	but read information regarding setup of the launchd files which
	are installed here:

	  /usr/local/Cellar/sleepwatcher/2.2/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist
	  /usr/local/Cellar/sleepwatcher/2.2/de.bernhard-baehr.sleepwatcher-20compatibility.plist

	These are the examples provided by the author.

	To have launchd start sleepwatcher at login:
	    ln -sfv /usr/local/opt/sleepwatcher/*.plist ~/Library/LaunchAgents
	Then to load sleepwatcher now:
	    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.sleepwatcher.plist