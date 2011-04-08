# Play #
	
## Setup ##	
	
### Get Play Framework ###

	git clone http://github.com/playframework/play.git
	cd play/framework/
	ant

### Update Play Framework ###

	cd play
	git pull origin master
	cd framework
	ant
	
### Setup Play ###

- make sure that Java is in your `PATH` (use `java -version`). Play will use the default Java or the one available at the `$JAVA_HOME` path if defined.
- I also setup `$PLAY_HOME` in my `.profile`
- make sure that play is in your `PATH`. For example via `export PATH=$PLAY_HOME:$PATH` 

I put the play framework in an `$SDKS` directory.

	# PLAY
	export PLAY_HOME=$SDKS/play
	export PATH=$PLAY_HOME:$PATH

# Usage #

## Create an app ##

	play new acmeapp
	
## Modules ##

### Installing modules ###

	play install gae
	play install siena
	
### Setup modules ###

	open acmeapp/conf/application.conf
	
	# Modules
	module.siena=${play.path}/modules/siena-1.3
	module.gae=${play.path}/modules/gae-1.4

## Usage ##
	
### Maven support ###

Install maven support via

	play install maven
	
Enable module in conf

	module.maven=${play.path}/modules/maven

Commands:

	play mvn:init
	
Installs play-parent project (a Maven pom project) into local Maven repository. Then creates the appropriate `pom.xml`. After this step, you can add your dependencies to `pom.xml`

	play mvn:update // or play mvn:up

Rretrieves all defined dependencies and then it copies them into your `app/lib` folder

	play mvn:refresh // or play mvn:re

Clears your `app/lib` folder first, then it executes play `mvn:up`

	play mvn:source // or play mvn:src

Retrieves all sources (if available) of defined dependencies and then it copies them into `app/lib` folder

### Eclipse ###

To transform a play application into a working Eclipse project, use the eclipsify command:

	play eclipsify
	
## FAQ/Problems ##

### Generating war results in `[Errno 63] File name too long` ###

You have to specify a path that is not inside you application when you 
generate the war archive. Otherwise it ends with an infinite 
recursion. 

	play war -o ../project.war
