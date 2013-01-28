# Redis #

## Installation ##

	brew install redis

Installation notes

	To have launchd start redis at login:
    ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
	Then to load redis now:
	    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
	Or, if you don't want/need launchctl, you can just run:
	    redis-server /usr/local/etc/redis.conf
	/usr/local/Cellar/redis/2.6.7: 9 files, 744K, built in 4 seconds
