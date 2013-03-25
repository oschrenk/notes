# Scala Build Tool #

	brew install sbt

Create Maven style project layout

	mkdir -p src/{main,test}/scala/
	touch build.sbt

A good starting point for a `build.sbt` with JUnit

	name := "FizzBuzz"

	version := "1.0"

	scalaVersion := "2.9.2"

	// JUnit itself is pulled in as a transitive dependency
	libraryDependencies += "com.novocode" % "junit-interface" % "0.10-M3" % "test"

The first start of sbt my take a while as plugins/dependencies are being downloaded.

## Create Eclipse project definitions ##

Use [sbteclipse](https://github.com/typesafehub/sbteclipse) to create Eclipseproject definitions.

- sbteclipse requires sbt `0.11.3-2` or `0.12`

Add `sbteclipse` to your plugin definition file. You can use either the global one at `~/.sbt/plugins/plugins.sbt` or the project-specific one at `$PROJECT_DIR/project/plugins.sbt`:

	addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "2.1.2")

I added it to my global config file.

Then

	sbt
	> eclipse

Finally in Eclipse use the Import Wizard to import Existing Projects into Workspace

## TDD with JUnit ##

Add the following dependency to your `build.sbt`

	// JUnit itself is pulled in as a transitive dependency
	libraryDependencies += "com.novocode" % "junit-interface" % "0.10-M3" % "test"<