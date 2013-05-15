# Play 2.0 #

## Multiple Environments ##

Scenario: You need to handle different configurations based on your environment, like one for development, staging and production.

This was fairly easy in Play 1.2 by using `%prod` or similar prefixes on your settings. Play 2.0 changed this behavior. Here are some approaches to, solve the problem.

### Specifying alternative configuration file ###

Per default Play 2 loads `conf/application.conf`. You can specify to load another configuration file but this will only work in production mode.

- `start -Dconfig.resource=prod.conf` Searches for configuration file in classpath, so you can put `prod.conf` file into the applications `conf/` directory to be picked up.
- `start -Dconfig.file=/opt/conf/prod.conf` Specifies a path.
- `start -Dconfig.url=http://conf.mycompany.com/conf/prod.conf` Loads configuration from URL.

As Play 2 allows for overriding variables and also to include other configuration files, the approach is to write configuration files for your environments, include the default configuration at the top of your file and then override settings suited for the environment.

	include "application.conf"

	# Override properties from application.conf
	my.setting = foobar

### Using environment variables ###

You can reference environment variables from your `application.conf` file:

	my.key = defaultvalue
	my.key = ${?MY_KEY_ENV}

The override field `my.key = ${?MY_KEY_ENV}` simply vanishes if there’s no value for `MY_KEY_ENV`, but if you set an environment variable `MY_KEY_ENV` for example, it would be used.

The disadvantage is that you would need to setup this environment variables on each machine your are trying to run the application.

### Using different modes with keys ###

There are [three different modes](https://github.com/playframework/Play20/blob/master/framework/src/play/src/main/java/play/Play.java#L18)

- `dev`. `play run`. For **each request** Play will
	- auto-reload, checking your project and recompile required sources
	- check your database schema
- `test`. `play test`.
- `prod`. `play start` When the **application starts** Play will
	- check your database schema

You can check the mode the application is running in, by calling `isProd()`, `isTest()`, and `isDev()` on the application object. For instance you can run

	(Play.application().isProd())
		? Play.application().configuration().getString("s3.bucket")
		: Play.application().configuration().getString("dev.s3.bucket");

### Mode based auto-configuration by extending the `Global` object ###

If you want to auto-configure your application, based on your mode you can change the behavior of Play bey extending the `Global` object.

A `Global` object in your project allows you to handle global settings for your application. This object **must be defined in the default (empty) package**.

	import java.io.File
	import play.api._
	import com.typesafe.config.ConfigFactory

	object Global extends GlobalSettings {
	  override def onLoadConfig(config: Configuration, path: File, classloader: ClassLoader, mode: Mode.Mode): Configuration = {
	    val modeSpecificConfig = config ++ Configuration(ConfigFactory.load(s"application.${mode.toString.toLowerCase}.conf"))
	    super.onLoadConfig(modeSpecificConfig, path, classloader, mode)
	  }
	}

This allows you to put `application.dev.conf`, `application.test.conf`, and `application.prod.conf` into your `./conf` with environment specific overrides (while keeping common settings in `application.conf`).

[source](http://stackoverflow.com/a/15937831/256853)