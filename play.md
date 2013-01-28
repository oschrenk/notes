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

Retrieves all defined dependencies and then it copies them into your `app/lib` folder

	play mvn:refresh // or play mvn:re

Clears your `app/lib` folder first, then it executes play `mvn:up`

	play mvn:source // or play mvn:src

Retrieves all sources (if available) of defined dependencies and then it copies them into `app/lib` folder

### Eclipse ###

To transform a play application into a working Eclipse project, use the eclipsify command:

	play eclipsify

## Deployment ##

### Environments ###

You can define your own environments in your `application.conf`

	%production.application.mode=prod

You can then change your settings according to your environment by prefixing it with `%production`. For instance you can define your own persistence layer when running in `production` environment.

	%production.db.url=jdbc:mysql://localhost/prod
	%production.db.driver=com.mysql.jdbc.Driver
	%production.db.user=root
	%production.db.pass=1515312

You can start your environment by passing it to play `start` or `play run` e.g..

	play start --%production

### Logging ###

You should define proper logging mechanisms

	log4j.rootLogger=ERROR, Rolling

	log4j.logger.play=INFO

	# Rolling files
	log4j.appender.Rolling=org.apache.log4j.RollingFileAppender
	log4j.appender.Rolling.File=application.log
	log4j.appender.Rolling.MaxFileSize=1MB
	log4j.appender.Rolling.MaxBackupIndex=100
	log4j.appender.Rolling.layout=org.apache.log4j.PatternLayout
	log4j.appender.Rolling.layout.ConversionPattern=%d{ABSOLUTE} %-5p ~ %m%n

### Front-end HTTP server ###

This setup is maybe more complicated than you are normally used to, but it is very nice to have. I usually deploy a dummy application, showing just a welcome page or something and manage the other instance via git. After that I can alternate between the instances when I deploy new features - very neat!

I'm using the Apache Server

	sudo apt-get apache2

You have to enable various modules by symlinking them

	/etc/apache2/mods-enabled$ la

	proxy.conf -> ../mods-available/proxy.conf
	proxy.load -> ../mods-available/proxy.load
	proxy_balancer.load -> ../mods-available/proxy_balancer.load
	proxy_http.load -> ../mods-available/proxy_http.load

	<VirtualHost mysuperwebapp.com:80>
	  ServerName mysuperwebapp.com
	  <Location /balancer-manager>
	    SetHandler balancer-manager
	    Order Deny,Allow
	    Deny from all
	    Allow from .mysuperwebapp.com
	  </Location>
	  <Proxy balancer://mycluster>
	    BalancerMember http://localhost:9999
	    BalancerMember http://localhost:9998 status=+H
	  </Proxy>
	  <Proxy *>
	    Order Allow,Deny
	    Allow From All
	  </Proxy>
	  ProxyPreserveHost On
	  ProxyPass /balancer-manager !
	  ProxyPass / balancer://mycluster/
	  ProxyPassReverse / http://localhost:9999/
	  ProxyPassReverse / http://localhost:9998/
	</VirtualHost>

The important part is balancer://mycluster. This declares a load balancer. The +H option means that the second Play application is on stand-by. But you can also instruct it to load-balance.

Every time you want to upgrade mysuperwebapp, here is what you need to do:

	play stop mysuperwebapp1

The load-balancer then forwards everything to mysuperwebapp2. In the meantime update mysuperwebapp1. Once you are done:

	play start mysuperwebapp1

You can now safely update mysuperwebapp2.

## FAQ/Problems ##

### Generating war results in `[Errno 63] File name too long` ###

You have to specify a path that is not inside you application when you
generate the war archive. Otherwise it ends with an infinite
recursion.

	play war -o ../project.war

## Resources ##

- [Offical Play Documentation](http://www.playframework.org/documentation/) It's one of the view open source documentation that is really, really useful. Use it.



