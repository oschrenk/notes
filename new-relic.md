# New Relic #

[New relic](http://newrelic.com/) enables Deep Application Monitoring

## Configuration ##

You can get a copy of the `newrelic.yml` pre-configured with your license key on the 'Account Settings' page.

### Environments ###

New Relic configuration supports various environments for applications to run in. For Rails applications, `RAILS_ENV` is used to determine the environment. For Java applications, pass `-Dnewrelic.environment <environment>`.

The default `newrelic.yml` comes with four different environments already setup: `development`, `test`, `production`, and `staging`.

### Logging ###

The log directory defaults to the same directory as the agent jar file resides in.

The log file name defaults to `newrelic_agent.log`.

Both can be in the configuration file.

## Setup ##

### Java ###

#### Installation ####

You can download a zip file provided on the homepage. It has the added benefit of not only containing the agent jar file but also a configuration file with the license key already set.

But I prefer to do it manually

	sudo mkdir -p /var/lib/newrelic
	sudo wget --output-document=/var/lib/newrelic/newrelic-agent-2.20.0.jar http://download.newrelic.com/newrelic/java-agent/newrelic-agent/2.20.0/newrelic-agent-2.20.0.jar
	ln -s /var/lib/newrelic/newrelic-agent-2.20.0.jar /var/lib/newrelic/newrelic-agent.jar

#### Configuration ####

The default is to place the `newrelic.yml` configuration file next to the jar next to the agent jar in the same directory. You can specify the absolute path to the configuration though by adding a system property `-Dnewrelic.config.file=PATH`.

The agent requires two pieces of information at startup the `license_key` and `app_name`.

### Play 2 ###

#### Production Mode Installation ####

Install the agent (see above).

Supply startup parameters to the start script

	./start -javaagent:path/to/newrelic.jar -Dnewrelic.bootstrap_classpath=true

#### Development Mode Installation ####

Install and configure the agent (see above).

Edit local play configuration

	vi $(dirname $(readlink -f $(which play)))/framework/build

Add the agent to the `JAVA_OPTS` command. If it doesn't exist, create it above the `java` line

	JAVA_OPTS="$JAVA_OPTS -javaagent:/opt/newrelic-agent.jar -Dnewrelic.bootstrap_classpath=true -Dnewrelic.environment=development"

Run the application

	play run

If you have multiple play applications this can get tricky.

### Tomcat ###

Basically the same setup as for Java. Tomcat applications runs via Apache Commons daemon (jsvc). Older versions don't support `-javaagent` option

Run

	jsvc -home $(readlink -f "$( which java )" | sed "s:bin/.*$::") --help

to check if the option exists.