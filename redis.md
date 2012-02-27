# Redis #

## Installation ##

	brew install redis

	If this is your first install, automatically load on login with:
	    mkdir -p ~/Library/LaunchAgents
	    cp /usr/local/Cellar/redis/2.4.8/io.redis.redis-server.plist ~/Library/LaunchAgents/
	    launchctl load -w ~/Library/LaunchAgents/io.redis.redis-server.plist

	If this is an upgrade and you already have the io.redis.redis-server.plist loaded:
	    launchctl unload -w ~/Library/LaunchAgents/io.redis.redis-server.plist
	    cp /usr/local/Cellar/redis/2.4.8/io.redis.redis-server.plist ~/Library/LaunchAgents/
	    launchctl load -w ~/Library/LaunchAgents/io.redis.redis-server.plist

	  To start redis manually:
	    redis-server /usr/local/etc/redis.conf

	  To access the server:
	    redis-cli