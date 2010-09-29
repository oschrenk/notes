# Java Developer Handbook #

## Basics ##

### Format a String ###

    String.format("First param %s, second param %s", firstParam, secondParam);

[Formatting Syntax](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Formatter.html#syntax)

     %[argument_index$][flags][width][.precision]conversion

## Class Hierarchy ##

Mostly taken from [http://www.javaworld.com/javaworld/javaqa/1999-08/01-qa-static2.html](http://www.javaworld.com/javaworld/javaqa/1999-08/01-qa-static2.html)

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

If you define a member class, that doesn't reference the surrounding top-level class, **do not** forget to declare it as `static` as otherwise each instance of this class has a reference to the surrounding class[p. 101][#Bloch:2002].

Nested top level classes are often usd to capsule objects representing components of the surrounding class (eg `Map.Entry`).

One **important note**: The `static` keyword does **not** do to a class declaration what it does to a variable or a method declaration. 
	 
## Garbage Collection ##

Java makes uses of Garbage Collection to remove objects from memory that are no longer being used. "_Being used_" normally meaning being _referenced_ by other objects. So this makes life for the developer easier, but it doesn’t mean that he doesn’t have to think about the lifecycle of an object. The developer could forget to de-reference an object no longer in use (caches and hash maps are a good candidate for such a mistake).

### Reference System ###

Java supports different types of references.

The _Strong Reference_

    StringBuffer buffer = new StringBuffer();

This is just your ordinary reference used every day, a new `StringBuffer` is created and the variable `buffer` holds a strong reference to it. Remember an object reachable through a chain of strong references is not eligible for garbage collection. This is expected behavior, you don’t want the garbage collector to destroy objects you are currently working with.

#### Weak Reference ####

A weak reference is a reference that is strong. It can’t force an object to remain in memory. You can create a weak reference like so:

    WeakReference<MyObject> myObject = new WeakReference<MyObject>(myObject);

At some time myObject might return `null`

#### finalize() ####

* every class inherits the `finalize()` method from `java.lang.Object`
* gets called from GC (Garbage collector), if there are no more references to the object
* `Object.finalize()` has no method body, but can be overridden
* should be used to close resources, for example

Every thrown `Exception` stops the `finalize()` method, but doesn’t stop the GC process

## Conventions ##

### JavaBeans ###

A _JavaBean_ has to conform to three properties

- parameter less constructor  
- class has (private ) member variables  
- public setter/getter method to access the values

Sometimes these objects are also called _POJOs_ (Plain Old Java Object), describing the fact that the don’t implement an interface, or extend another class, they are just a normal object.

## Documentation ##

### JavaDoc ###

#### Proposed Tags ####

`@category` For logically grouping classes, methods, fields together

## Interview Questions ##

### Servlet Engine ###

There is an form with a button Submit,
a servlet and a relational database with the table Customers. The users enters a First and Last names of the customer, presses the button Submit, and you need to write the code that would return the detailed data about this customer. Explain _IN with the values encoded as key/value pairs, if the form uses `POST` the data is encoded in the request body.

Details like translating the address via , opening a against the patterns defined in the `web.xml` to forward the request to the servlet responsible for answering it. The servlet engine spawns a new thread and therefore starts the lifecycle of a servlet.

The servlet lifecycle consists of the following steps:

1.  The servlet class is loaded by the container during start-up.
2.  The container calls the `init()` method. This method initializes the servlet and must be called before the servlet can service any requests. In the entire life of a servlet, the init() method is called only once.
3.  After initialization, the servlet can service client requests. Each request is serviced in its own separate thread. The container calls the `service()` method of the servlet for every request. The `service()` method determines the kind of request being made and dispatches it to an appropriate method to handle the request. The developer of the servlet must provide an implementation for these methods. If a request for a method that is not implemented by the servlet is made, the method of the parent class is called, typically resulting in an error being returned to the requester.  
Finally, the container calls the `destroy()` method that takes the servlet out of service. The destroy() method, like init(), is called only once in the lifecycle of a servlet.

## Problems/##

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
ant script. Some people had the [same
problem](http://blog.anthonychaves.net/java/2006/12/01/solution-for-classnotfoundexception-with-junit-and-ant/comment-page-1#comment-9478)

> Apparently Ant expects all the classes to be in Jar files rather than in directories on the file system. A few tweaks to my build.xml file and it looked like this: build.xml Now I’m creating two jar files, one for the application and one for the unit tests. After the unit test jar is created I run them in the JUnit task after setting the classpath with the test jar file. I would have liked to put off creating the application jar file until I knew the JUnit tests passed but that is not an option at this point. I don’t feel strongly enough to make it an option either. I have a 2.4 GHz Pentium 4 computer, I can afford to make a few extra jar files.

The hint is to pack the classes into a jar, and add the jar to the classpath and not the directory whee the classes reside.

## Appendix ##

### FAQ/Problems ###

#### Out of Heap Space ####

Just add the following VM parameter

- `-Xms<size>` set initial Java heap size
- `-Xmx<size>`  set maximum Java heap size

For example `-Xms512m -Xmx768m` whereas the m would stand for megabyte.

JVM starts with `-Xms` amount of memory for the heap (storing objects etc.) and can grow to a maximum of `-Xmx` amount of memory. 

### JavaDoc ###

[Java SE 1.3](http://java.sun.com/j2se/1.3/docs/api/)  
[Java SE 1.4](http://java.sun.com/j2se/1.4.2/docs/api/)  
[Java SE 1.5](http://java.sun.com/j2se/1.5.0/docs/api/)  
[Java SE 6](http://java.sun.com/javase/6/docs/api/)

[Java EE 1.2](http://java.sun.com/j2ee/sdk_1.2.1/techdocs/api/index.html)  
[Java EE 1.3](http://java.sun.com/j2ee/sdk_1.3/techdocs/api/)  
[Java EE 1.4](http://java.sun.com/j2ee/1.4/docs/api/index.html)  
[Java EE 5](http://java.sun.com/javaee/5/docs/api/)  
[Java EE 6](http://java.sun.com/javaee/6/docs/api/)


[#Doe:2006]: Joshua Bloch. *Effektiv Java Programmieren*.  Addison-Wesley, 2002.
