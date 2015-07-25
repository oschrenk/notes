Logging
--------------------

Make sure you have a proper logging with

- code context
- time/duration of your action
- transaction id
- maintain across machines

Do that by [setting thread name](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html#setName(java.lang.String))

```java
Thread.currentThread().setName(...)

// "id: ABCDE, type: analzye, queue: PROD, tid: 12345, created: 2015-04-23T12:18" ...
```

As a last line of defense you can do *[Global Exception Handling](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html#setDefaultUncaughtExceptionHandler-java.lang.Thread.UncaughtExceptionHandler-)*.

```java
// default

// per thread
Thread.setUncaughtExceptionHandler(new Handler());

class Handler implements Thread.UncaughtExceptionHandler {
  public void uncaughtException(Thread t, Throwable e) {
    System.out.println("foo");
  }
}
```

jstack
--------------------

[jstack](http://docs.oracle.com/javase/7/docs/technotes/tools/share/jstack.html) is a great tool to print stack traces of a running jvm.

```shell
$ jstack <pid>
$ jstack server@remote
```

But this happens after the fact, the server might already be stuck, leaving you with no valuable information. The idea is to define application boundary, and shell out to [jstack preemptively](https://github.com/takipi/jstack) from within your application, if your application behaviour doesn't fall within these boundaries.

btrace
--------------------

[btrace](https://github.com/jbachorik/btrace) can attach itself to jvm and extract information, like tracing method calls or constructor invocations. It even offers statsd integration. It is limited to read access though.

```shell
git clone git@github.com:jbachorik/btrace.git
cd btrace
./gradlew build
./gradlew buildDistributions


Java Agent
--------------------

If you need more power and inject or transform existing code in the JVM you can do that by writing your own Java agent. That means writing your own bytecode transformer though. The appropriate ASM bytecode outline plugin for your IDE is of great help there.
