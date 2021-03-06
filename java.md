# Java Developer Handbook #

## Concepts ##

### Java Class Loader ###

The Java Class Loader is a part of the JRE that dynamically loads Java Classes into the Java Virtual Machine. Usually classes are loaded on demand (lazy initialization). The Java Run Time does not need to know about files and file systems because of class loaders. They are an abstraction (or indirection) between a resource name and its actual location.

In Java a library, a collection of object code, is typically packaged in Jar files. The class loader is responsible for locating libraries, reading their contents and loading the classes.

When the JVM is started, three class loaders are used.

1. Bootstrap class loader. Loads core Java Libraries (in `$JAVA_HOME/lib`). Written in native code.
2. Extensions class loader. Loads extensions code (in `$JAVA_HOME/lib/ext` or other directories specified in `java.ext.dirs` system property). Implemented by the `sun.misc.Launcher$ExtClassLoader` class.
3. System class loader. Loads code found on `java.class.path`, which maps to the system `CLASSPATH` variable. Implemented by the `sun.misc.Launcher$AppClassLoader` class.

All class loaders are of type `java.lang.ClassLoader`

#### Loading Properties and Configuration Files ####

In part taken from [Java World, Smartly load your properties](http://www.javaworld.com/javaworld/javaqa/2003-08/01-qa-0808-property.html)

Say No to `java.io`.
	- Absolute filenames aren't portable
	- Relative file	names are better but are resolved to JVM's current directory, which details can change (depending on JVM setup, deployment context, servlet container, ...)

Use classloader instead. They are an abstraction between a resource name and its actual location.

You can get at `some/pkg/resource.properties` programmatically from your Java code in several ways:

	ClassLoader.getResourceAsStream ("some/pkg/resource.properties");
	Class.getResourceAsStream ("/some/pkg/resource.properties");
	ResourceBundle.getBundle ("some.pkg.resource");

Additionally, if the code is in a class within a `some.pkg` Java package, then the following works as well:

	  Class.getResourceAsStream ("resource.properties");

| Method | Parameter format | Lookup failure behavior | Usage example |
| :---- | :---- | :---- | :---- |
| `ClassLoader. getResourceAsStream()` | `/`-separated; no leading `/` (all names are absolute) | Silent (returns `null`) | `this.getClass(). getClassLoader(). getResourceAsStream ("some/pkg/resource.properties")` |
| `Class. getResourceAsStream()` | `/`-separated; leading `/` indicates absolute names; others are relative to the class's package | Silent (returns `null`) | `this.getClass(). getResourceAsStream ("resource.properties")` |
| `ResourceBundle. getBundle()` | `.`-separated names; all names are absolute; `.properties` suffix implied | Throws unchecked `java.util.MissingResourceException` | `ResourceBundle. getBundle ("some.pkg.resource") `|

### Class Hierarchy ###

Mostly taken from [Java World, Static class declarations](http://www.javaworld.com/javaworld/javaqa/1999-08/01-qa-static2.html)

**Top level classes** are classes is declared at the top level of a package, declared in its own file with the same name as the class name. A top level class is by definition top-level, adding a `static`keyword has no point and is in fact an error that the compiler will detect.
**Inner classes** are, as the name suggests, declared within in top-level classes. An inner class cane be one of the following four types:

1. **Anonymous**
Anonymous classes are declared and instantiated within the same statement. They do not have names, and they can be instantiated only once.

	okButton.addActionListener( new ActionListener(){
		public void actionPerformed(ActionEvent e){
			dispose();
		}
	});

You will often find them in GUI related classes. As an anonymous class doesn't have a class declaration, it can't use the `static` keyword.

2. **Local** Local classes are the same as local variables, in the sense that they're created and used inside a block. Once you declare a class within a block, it can be instantiated as many times as you wish within that block. Like local variables, local classes aren't allowed to be declared `public`, `protected`, `private`, or `static`.

	//some code block ... {
		class ListListener implements ItemListener {
			List list;
			public ListListener(List l) {
				list = l;
			}
			public void itemStateChanged(ItemEvent e) {
				doSomething(list);
			}
		}
		List list1 = new List();
		list1.addItemListener(new ListListener(list1));
	}

3. **Member**
Member classes are defined within the body of a class. You can use member classes anywhere within the body of the containing class. You declare member classes when you want to use variables and methods of the containing class without explicit delegation.

The member class is the only class that you can declare `static`. When you declare a member class, you can instantiate that member class only within the context of an object of the outer class in which this member class is declared. If you want to remove this restriction, you declare the member class a `static` class.

When you declare a member class with a `static` modifier, it becomes a nested top-level class and can be used as a normal top-level class as explained above.

4. **Nested Top-Level**
A nested top-level class is a member classes with a `static` modifier. A nested top-level class is just like any other top-level class except that it is declared within another class or interface. Nested top-level classes are typically used as a convenient way to group related classes without creating a new package.

If your main class has a few smaller helper classes that can be used outside the class and make sense only with your main class, it's a good idea to make them nested top-level classes. To use the nested top-level class, write: `TopLevelClass.NestedClass`.

If you define a member class, that doesn't reference the surrounding top-level class, **do not** forget to declare it as `static` as otherwise each instance of this class has a reference to the surrounding class [p. 101][#Bloch:2002].

Nested top level classes are often used to capsule objects representing components of the surrounding class (eg. `Map.Entry`).

One **important note**: The `static` keyword does **not** do to a class declaration what it does to a variable or a method declaration.

### Garbage Collection ###

Java makes uses of Garbage Collection to remove objects from memory that are no longer being used. "_Being used_" normally meaning being _referenced_ by other objects. So this makes life for the developer easier, but it doesn’t mean that he doesn’t have to think about the lifecycle of an object. The developer could forget to de-reference an object no longer in use (caches and hash maps are a good candidate for such a mistake).

### Reference System ###

Java supports different types of references.

The _Strong Reference_

    StringBuffer buffer = new StringBuffer();

This is just your ordinary reference used every day, a new `StringBuffer` is created and the variable `buffer` holds a strong reference to it. Remember an object reachable through a chain of strong references is not eligible for garbage collection. This is expected behavior, you don’t want the garbage collector to destroy objects you are currently working with.

The _Weak Reference_

A weak reference is a reference that is strong. It can’t force an object to remain in memory. You can create a weak reference like so:

    WeakReference<MyObject> myObject = new WeakReference<MyObject>(myObject);

At some time myObject might return `null`

#### finalize() ####

* every class inherits the `finalize()` method from `java.lang.Object`
* gets called from GC (Garbage collector), if there are no more references to the object
* `Object.finalize()` has no method body, but can be overridden
* should be used to close resources, for example

Every thrown `Exception` stops the `finalize()` method, but doesn’t stop the GC process

### JavaBeans ###

Java and the programming world itself thrive on conventions. A _JavaBean_ has to conform to three properties

- parameter less constructor
- class has (private ) member variables
- public setter/getter method to access the values

Sometimes these objects are also called _POJOs_ (Plain Old Java Object), describing the fact that the don’t implement an interface, or extend another class, they are just a normal object.

## Basics ##

### Format a String ###

    String.format("First param %s, second param %s", firstParam, secondParam);

[Formatting Syntax](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Formatter.html#syntax)

     %[argument_index$][flags][width][.precision]conversion

## Troubleshooting ###

### Missing Java 1.4 on OS X ###

People from [OneSwarm](http://oneswarm.cs.washington.edu/) were so
kind providing a 1.4.2 version for Snow Leopard

    curl -o java.1.4.2-leopard.tar.gz http://www.cs.washington.edu/homes/isdal/snow_leopard_workaround/java.1.4.2-leopard.tar.gz
    tar -xvzf java.1.4.2-leopard.tar.gz
    sudo mv 1.4.2 /System/Library/Frameworks/JavaVM.framework/Versions/1.4.2
    cd /System/Library/Frameworks/JavaVM.framework/Versions/
    sudo ln -s 1.4.2 1.4

### Running junit from ant causes `ClassNotFoundException` ###

I ran into some problems when trying to calling JUnit tests from an
ant script. Some people had the [same problem](http://blog.anthonychaves.net/java/2006/12/01/solution-for-classnotfoundexception-with-junit-and-ant/comment-page-1#comment-9478)

> Apparently Ant expects all the classes to be in Jar files rather than in directories on the file system. A few tweaks to my build.xml file and it looked like this: build.xml Now I’m creating two jar files, one for the application and one for the unit tests. After the unit test jar is created I run them in the JUnit task after setting the classpath with the test jar file. I would have liked to put off creating the application jar file until I knew the JUnit tests passed but that is not an option at this point. I don’t feel strongly enough to make it an option either. I have a 2.4 GHz Pentium 4 computer, I can afford to make a few extra jar files.

The hint is to pack the classes into a jar, and add the jar to the classpath and not the directory whee the classes reside.

### Out of Heap Space ####

Just add the following VM parameter

- `-Xms<size>` set initial Java heap size
- `-Xmx<size>`  set maximum Java heap size

For example `-Xms512m -Xmx768m` whereas the m would stand for megabyte.

JVM starts with `-Xms` amount of memory for the heap (storing objects etc.) and can grow to a maximum of `-Xmx` amount of memory.

### JavaDoc ###

[Java SE 1.3](http://download.oracle.com/javase/1.3/docs/api/)
[Java SE 1.4](http://download.oracle.com/javase/1.4.2/docs/api/)
[Java SE 1.5](http://download.oracle.com/javase/1.5.0/docs/api/)
[Java SE 6](http://download.oracle.com/javase/6/docs/api/)
[Java SE 7](http://download.oracle.com/javase/7/docs/api/)

[Java EE 1.2](http://download.oracle.com/javaee/1.2.1/api/)
[Java EE 1.3](http://download.oracle.com/javaee/1.3/api/)
[Java EE 1.4](http://download.oracle.com/javaee/1.4/api/)
[Java EE 5](http://download.oracle.com/javaee/5/api/)
[Java EE 6](http://download.oracle.com/javaee/6/api/)

[#Bloch:2002]: Joshua Bloch. *Effektiv Java Programmieren*.  Addison-Wesley, 2002.
