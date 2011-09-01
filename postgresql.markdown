# PostgreSQL #

## Installation on OSX ##

	brew install postgresql
	[...]
	
	==> Caveats
	If builds of PostgreSQL 9 are failing and you have version 8.x installed,
	you may need to remove the previous version first. See:
	  https://github.com/mxcl/homebrew/issues/issue/2510

	To build plpython against a specific Python, set PYTHON prior to brewing:
	  PYTHON=/usr/local/bin/python  brew install postgresql
	See:
	  http://www.postgresql.org/docs/9.0/static/install-procedure.html


	If this is your first install, create a database with:
	  initdb /usr/local/var/postgres

	If this is your first install, automatically load on login with:
	  mkdir -p ~/Library/LaunchAgents
	  cp /usr/local/Cellar/postgresql/9.0.4/org.postgresql.postgres.plist ~/Library/LaunchAgents/
	  launchctl load -w ~/Library/LaunchAgents/org.postgresql.postgres.plist

	If this is an upgrade and you already have the org.postgresql.postgres.plist loaded:
	  launchctl unload -w ~/Library/LaunchAgents/org.postgresql.postgres.plist
	  cp /usr/local/Cellar/postgresql/9.0.4/org.postgresql.postgres.plist ~/Library/LaunchAgents/
	  launchctl load -w ~/Library/LaunchAgents/org.postgresql.postgres.plist

	Or start manually with:
	  pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start

	And stop with:
	  pg_ctl -D /usr/local/var/postgres stop -s -m fast


	Some machines may require provisioning of shared memory:
	  http://www.postgresql.org/docs/current/static/kernel-resources.html#SYSVIPC

	If you want to install the postgres gem, including ARCHFLAGS is recommended:
	    env ARCHFLAGS="-arch x86_64" gem install pg

## Usage ##

List databases

	\l
	
Drop database

	DROP DATABASE name;