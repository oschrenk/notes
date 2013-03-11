# Scala #

- statically typed
- it is functional: functions are values
- it is object oriented: every value is an object
- compiles to bytecode for the JVM and CLR

Comparison to Java

	// Java/C#
	public static void main(String[] args) //Body

	// Scala
	def main(args: Array[String]) = //Body

- the concept of static methods (or variables or classes) doesn't exist in Scala. In Scala everything is an object.
- Scala has an `object` construct with which we can declare singletons. In other word our main method is an instance method on a singleton object that is automatically instantiated for us.
- no return type. In fact there is a return type, Unit which is similar to void, but it’s inferred by the compiler. Should we want to we can explicitly specify the return type by putting a colon and the type after the parameters
- default access level is public.

## Installation ##

	brew install scala

### Eclipse IDE ##

- [Update Site](http://download.scala-ide.org/sdk/e38/scala210/dev/site/) for Eclipse 4.2 and Scala 2.10

## Scala Build Tool ##

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

### Create Eclipse project definitions ###

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

## Troubleshooting ##

### Heap settings ###

Upon installation the Scala IDE plugin runs a settings diagnostics and warns me about my maximum heap size.

	Current Maximum heap size: 455M
	Warning: recommended value is at least 1024M

and links to [Instructions for changing heap size](http://wiki.eclipse.org/FAQ_How_do_I_increase_the_heap_size_available_to_Eclipse%3F).

For example, the following command would run Eclipse with a heap size of 256MB:

	eclipse [normal arguments] -vmargs -Xmx256M

You can change the default startup options in your `eclipse.ini`. As I'm on a Mac I have to right-click (or Ctrl+click) on the Eclipse executable in Finder, choose Show Package Contents, and then locate `eclipse.ini` in the MacOS folder under `Contents`.

I changed my settings from

	-Xms40m
	-Xmx512m

to

	-Xms1024m
	-Xmx1024m

### Unable to find a scala library. Please add the scala container or a scala library jar to the build path. ###

After creating a new Scala project using the Scala IDE. the project complains

	Unable to find a scala library. Please add the scala container or a scala library jar to the build path.

From the [FAQ](http://scala-ide.org/docs/user/faq.html)

> The simplest thing is to add the Scala library container: right-click on the project in the Package Explorer view, then in the context menu select Build Path → Add Libraries..., and add the Scala Library.

### Can't run the Application ##

If *any* file can't be compiled, it seems that Scala IDE can't run the application. I had the problem that an old file was still registered somewhere and although it didn't exist on the file system, even the other files couldn't be executed.

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