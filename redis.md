# Redis #

## Installation ##

	brew install redis

Installation notes

	If this is your first install, automatically load on login with:
    mkdir -p ~/Library/LaunchAgents
    cp /usr/local/Cellar/redis/2.6.4/homebrew.mxcl.redis.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist

	If this is an upgrade and you already have the homebrew.mxcl.redis.plist loaded:
	    launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
	    cp /usr/local/Cellar/redis/2.6.4/homebrew.mxcl.redis.plist ~/Library/LaunchAgents/
	    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist

	  To start redis manually:
	    redis-server /usr/local/etc/redis.conf

	  To access the server:
	    redis-cli
