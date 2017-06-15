# Scala Build Tool #

[sbt](http://www.scala-sbt.org/) is a build tool for Scala and Java projects

	brew install sbt

## Configuration ##

### Global settings ###

Settings that should be applied to all projects can go in `~/.sbt/0.13/global.sbt` (or any file in `~/.sbt/0.13` with a `.sbt` extension). Plugins that are defined globally in `~/.sbt/0.13/plugins/` are available to these settings.

### Global plugins ###

Any `.sbt` file in `~/.sbt/0.13/plugins/` is picked up. I prefer to create one file per plugin eg `~/.sbt/0.13/plugins/ensime.sbt`

To add a plugin globally, create `~/.sbt/0.13/plugins/<id>.sbt` containing the dependency definitions eg

```
addSbtPlugin("org.ensime" % "sbt-ensime" % "1.12.5")
```

### Aliases and .sbtrc ###

If you want to execute commands before sbt starts up and the project is loaded, you can create a `.sbtrc` file. This is useful to define aliases, for example:

```
alias c=compile
alias tc=test:compile
```

First commands in `~/.sbtrc` (if exists) are evaluated and then commands in the local directory

## Project structure ##

### Sources ###

sbt uses the same directory structure as Maven for source files by default (all paths are relative to the base directory) and `project` directory to hold files specific to your build process.

```
mkdir -p project src/{main,test}/{scala,resources}
```

### build.sbt ###

The build definition is described in `build.sbt` (actually any files named `*.sbt`) in the projects base directory.

It is *recommended* to use a `build.sbt` file in the project root to specify your build and have `project/*.scala` to define helper objects.


## Build definition

It is *recommended* to specify the version of SBT for a project

```
echo "sbt.version=0.13.15" > project/build.properties
```

For most small projects just having the `build.sbt` should suffice.

A very simple `build.sbt`:

```
lazy val root = (project in file("."))
  .settings(
    name := "Hello",
    scalaVersion := "2.12.1"
  )
```

## Dependency Managment ##

The `%%` is for cross-compiled libraries (which are compiled against multiple Scala versions) and automatically adds the Scala version to the artifact id.

## Usage ##

The first start of sbt my take a while as plugins and dependencies are being downloaded or compiled.

## Plugins ##

Useful plugins

- [sbt-dependency-graph](https://github.com/jrudolph/sbt-dependency-graph) - display an ASCII graph of hierarchical direct and transitive dependencies
- [sbt-dirty-money](https://github.com/sbt/sbt-dirty-money) - clean your Ivy2 cache. If you use publish-local to test plugins
- [sbt-release](https://github.com/sbt/sbt-release) - provides a customizable release process that you can add to your project.

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

## Appendix

### Terminology

* **base directory** is the directory containing the project. So if you created a project hello containing `hello/build.sbt`, `hello` is your base directory.

## Sources ##

- [scala-sbt.org/docs](http://www.scala-sbt.org/0.12.4/docs/)
- [Mike's Scala Notes](http://scala.micronauticsresearch.com/sbt)
