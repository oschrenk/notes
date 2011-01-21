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
	
### Setup Play#

- make sure that Java is in your @path@ (use @java -version). Play will use the default Java or the one available at the @$JAVA_HOME@ path if defined.
- I also setup $PLAY_HOME in my @.profile@
- make sure that play is in your @path@. For example via @export PATH=$PLAY_HOME:$PATH@ 


I put the play framework in an `$SDKS` directory.

	# PLAY
	export PLAY_HOME=$SDKS/play
	export PATH=$PLAY_HOME:$PATH

# Usage #

## Create an app ##

	play new acmeapp
	
## Get modules

	play install gae
	play install siena
	
## Setup modules

	open acmeapp/conf/application.conf
	
	# Modules
	module.siena=${play.path}/modules/siena-1.3
	module.gae=${play.path}/modules/gae-1.4