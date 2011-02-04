# MongoDB #

	brew install mongodb
	==> Caveats
	If this is your first install, automatically load on login with:
	    cp /usr/local/Cellar/mongodb/1.6.5-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents
	    launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist

	If this is an upgrade and you already have the org.mongodb.mongod.plist loaded:
	    launchctl unload -w ~/Library/LaunchAgents/org.mongodb.mongod.plist
	    cp /usr/local/Cellar/mongodb/1.6.5-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents
	    launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist

	Or start it manually:
	    mongod run --config /usr/local/Cellar/mongodb/1.6.5-x86_64/mongod.conf
	