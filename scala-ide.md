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

After creating a new Scala project using the Scala IDE the project complains

	Unable to find a scala library. Please add the scala container or a scala library jar to the build path.

From the [FAQ](http://scala-ide.org/docs/user/faq.html)

> The simplest thing is to add the Scala library container: right-click on the project in the Package Explorer view, then in the context menu select Build Path → Add Libraries..., and add the Scala Library.

### Can't run the Application ##

If *any* file can't be compiled, it seems that Scala IDE can't run the application. I had the problem that an old file was still registered somewhere and although it didn't exist on the file system, even the other files couldn't be executed.
