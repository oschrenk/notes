# Sleepwatcher #

	brew install sleepwatcher

Consult the README in `/usr/local/Cellar/sleepwatcher/2.2/ReadMe.rtf`. Ignore information about installing the binary and man page,	but read information regarding setup of the `launchd` files which	are installed here

	  /usr/local/Cellar/sleepwatcher/2.2/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist
	  /usr/local/Cellar/sleepwatcher/2.2/de.bernhard-baehr.sleepwatcher-20compatibility.plist

These are the examples provided by the author. I installed the one for a local user. To have launchd start sleepwatcher at login:

	ln -sfv /usr/local/opt/sleepwatcher/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist ~/Library/LaunchAgents/com.oschrenk.sleepwatcher.plist

Then to load sleepwatcher now:

    launchctl load ~/Library/LaunchAgents/com.oschrenk.sleepwatcher.plist

The launchd agent executes
- `~/.sleep` when the machine goes to sleep
- `~/.wakeup` when the machine wakes up