## Google App Engine Java SDK

	svn checkout http://googleappengine.googlecode.com/svn/trunk/java appengine-java-sdk-svn
	ln -s appengine-java-sdk-svn/ gae

	# GAE
	export GAE_HOME=$SDKS/gae
	export PATH=$GAE_HOME/bin:$PATH
	
## Get Play Framework

	bzr branch lp:play/1.1 play-1.1-bzr
	ln -s play-1.1-bzr/ play
	cd play/framework/
	ant
	
or

	git clone http://github.com/playframework/play.git
	cd play/framework/
	ant

## Update Play Framework ##

	cd play
	bzr merge
	cd framework
	ant
	
or

	cd play
	git pull origin master
	cd framework
	ant
	
## Setup Play

- make sure that Java is in your @path@ (use @java -version). Play will use the default Java or the one available at the @$JAVA_HOME@ path if defined.
- I also setup $PLAY_HOME in my @.profile@
- make sure that play is in your @path@. For example via @export PATH=$PLAY_HOME:$PATH@ 

	# PLAY
	export PLAY_HOME=$SDKS/play
	export PATH=$PLAY_HOME:$PATH

## Create the app

	play new acmeapp
	
## Get modules

	play install gae
	play install siena
	
## Setup modules

	open acmeapp/conf/application.conf
	
	# Modules
	module.siena=${play.path}/modules/siena-1.1
	module.gae=${play.path}/modules/gae-1.0.2

## Register your application with GAE


## Run once to create neccesary stub files ##

Add your GAE application ID to @acmeapp/war/WEB-INF/appengine-web.xml@

## Deployment ##

### Local ###

	play war myappname -o myappname-war
	$GAE_HOME/bin/dev_appserver myappname-war

### Live ###

	play war myappname -o ../myappname-war
	$GAE_HOME/bin/appcfg update myappname-war/

# Managing GAE Application #

http://code.google.com/appengine/docs/java/tools/uploadinganapp.html
