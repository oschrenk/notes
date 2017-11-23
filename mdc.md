# Mapped Diagnostic Contexts (MDC)

> Most real-world distributed systems need to deal with multiple clients simultaneously. In a typical multithreaded implementation of such a system, different threads will handle different clients.

When it comes to logging one approach would be then to instantiate a logger for each client. This is a lot of overhead.

A different technique is stamping each log request. Neil Harrison described this method in the book Patterns for Logging Diagnostic Messages in Pattern Languages of Program Design 3, edited by R. Martin, D. Riehle, and F. Buschmann (Addison-Wesley, 1997).

Mapped Diagnostic Contexts (MDC) is an implementation of that idea.

Let's look at SLF4Js implementation of this:
```
package org.slf4j;

public class MDC {
  //Put a context value as identified by key
  //into the current thread's context map.
  public static void put(String key, String val);

  //Get the context identified by the key parameter.
  public static String get(String key);

  //Remove the context identified by the key parameter.
  public static void remove(String key);

  //Clear all entries in the MDC.
  public static void clear();
}
```

Usage would be:

```
package chapters.mdc;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.slf4j.MDC;

import ch.qos.logback.classic.PatternLayout;
import ch.qos.logback.core.ConsoleAppender;

public class SimpleMDC {
  static public void main(String[] args) throws Exception {
    Logger logger = LoggerFactory.getLogger(SimpleMDC.class);
    MDC.put("first", "Richard");
    MDC.put("last", "Nixon");
    logger.info("I am not a crook.");
    logger.info("Attributed to the former US president. 17 Nov 1973.");
  }
}
```

Part of the logger configuration is this

```
<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
  <layout>
    <Pattern>%X{first} %X{last} - %m%n</Pattern>
  </layout>
</appender>
```

Given that MDC information is managed on a per thread basis, each thread will have its own copy of the MDC.

> Normally, a put() operation should be balanced by the corresponding remove() operation. Otherwise, the MDC will contain stale values for certain keys. We would recommend that whenever possible, remove() operations be performed within finally blocks, ensuring their invocation regardless of the execution path of the code.

> The MDC manages contextual information on a per thread basis. A child thread automatically inherits a copy of the mapped diagnostic context of its parent. Typically, while starting to service a new client request, the developer will insert pertinent contextual information, such as the client id, client's IP address, request parameters etc. into the MDC. Logback components, if appropriately configured, will automatically include this information in each log entry.



## Resources

https://logback.qos.ch/manual/mdc.html
