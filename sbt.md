# Scala Build Tool #

[sbt](http://www.scala-sbt.org/) is a build tool for Scala and Java projects

	brew install sbt

## Global Setup ##

### Multiple versions ###

You can have `~/.sbt-0.11/plugins/plugins.sbt` for SBT `0.11` and `~/.sbt-0.12/plugins/plugins.sbt` for SBT `0.12`. That's crucial when working with two or more versions of sbt.

### .sbtrc ###

Each line in `.sbtrc` and `~/.sbtrc` is evaluated as a command before the project is loaded.

## Project Setup ##

sbt supports two types of project definitions, one using a `build.sbt` file in the project root, and the other is having a `Build.scala` in a `project` subdirectory.

You can mix both but the settings in `.sbt` will take precedence.

### Project Layout ###

The preferred build style with `Build.scala` lives in `project` directory. In general sources should be organized like a maven project.

	mkdir -p project src/{main,test}/{scala,resources}

It is *recommended*  to specify the version of SBT that works best

	echo "sbt.version=0.12.4" > project/build.properties

### build.sbt ###

For most small projects just having a `build.sbt` should suffice.

	touch build.sbt

A very simple `build.sbt`

	name := "FizzBuzz"

	version := "1.0"

	scalaVersion := "2.10.3"

### project/Build.scala ###

### Mixing build.sbt and project/Build.scala ###

- In `.scala` files, you can add settings to `Build.settings` for sbt to find, and they are automatically build-scoped.
- In `.scala` files, you can add settings to Project.settings for sbt to find, and they are automatically project-scoped.
- Any Build object you write in a `.scala` file will have its contents imported and available to `.sbt` files.
- The settings in `.sbt` files are appended to the settings in `.scala` files.
- The settings in `.sbt` files are project-scoped unless you explicitly specify another scope.

## Dependency Managment ##

The `%%` is for cross-compiled libraries (which are compiled against multiple Scala versions) and automatically adds the Scala version to the artifact id.

## Usage ##

The first start of sbt my take a while as plugins and dependencies are being downloaded or compiled.

### Running the Scala Interpreter ###

Start the `sbt` command prompt

	sbt

Start the Scala interpreter inside `sbt` (it will download some dependencies)

	> console
	...
	scala >

In order to quit the interpreter and get back to `sbt`, type `ctrl-d`.

## Plugins ##

- [sbt-dependency-graph](https://github.com/jrudolph/sbt-dependency-graph) - display an ASCII graph of hierarchical direct and transitive dependencies
- [sbt-dirty-money](https://github.com/sbt/sbt-dirty-money) - clean your Ivy2 cache. If you use publish-local to test plugins
- [sbt-release](https://github.com/sbt/sbt-release) - provides a customizable release process that you can add to your project.

### Eclipse ###

Use [sbteclipse](https://github.com/typesafehub/sbteclipse) to create Eclipseproject definitions.

- sbteclipse requires sbt `0.11.3-2` or `0.12`

Add `sbteclipse` to your plugin definition file. You can use either the global one at `~/.sbt/plugins/plugins.sbt` or the project-specific one at `$PROJECT_DIR/project/plugins.sbt`:

	addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "2.1.2")

I added it to my global config file.

Then

	sbt
	> eclipse

Finally in Eclipse use the Import Wizard to import Existing Projects into Workspacest

## FAQ ##

### Skip tests for assembly ###

	lazy val appAssemblySettings = assemblySettings ++ Seq(test in assembly := {})

### Configure test output ###

You can enhance the test output by supplying some configuration

	testOptions in Test += Tests.Argument("-oDSFW")

The options:

- `-o` required prefix to pass options
- `D` show durations
- `S` show short stack traces
- `F` show full stack traces
- `W` without color

Or if you are using more than one test framework, like this:

	testOptions in Test += Tests.Argument(TestFrameworks.ScalaTest, "-oD")

You can also pass arguments for individual runs by placing them after `--`, like this:

	> test-only org.acme.RedSuite -- -oD

### java.lang.OutOfMemoryError: PermGen space ###

When compiling compiler-interface for Scala 2.10.1-RC3 using sbt I got

	[info] 'compiler-interface' not yet compiled for Scala 2.10.1-RC3. Compiling...
	sbt appears to be exiting abnormally.
	  The log file for this session is at /var/folders/ld/w3p3zcjj4j10q_wnw_vt7rm40000gp/T/sbt3740649230551384318.log
	java.lang.OutOfMemoryError: PermGen space

	Error during sbt execution: java.lang.OutOfMemoryError: PermGen space

Scala needs a lot of memory

	echo "export SBT_OPTS=-XX:MaxPermSize=256M" > $HOME/.sbtconfig

helped.

## Sources ##

- [scala-sbt.org/docs](http://www.scala-sbt.org/0.12.4/docs/)
- [Mike's Scala Notes](http://scala.micronauticsresearch.com/sbt)
