# Google Apps Engine #

## Java SDK ##

	svn checkout http://googleappengine.googlecode.com/svn/trunk/java appengine-java-sdk-svn
	ln -s appengine-java-sdk-svn/ gae

	# GAE
	export GAE_HOME=$SDKS/gae
	export PATH=$GAE_HOME/bin:$PATH
	
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