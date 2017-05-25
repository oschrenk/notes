# Scala Compiler

Useful flags

```
scalacOptions ++= Seq(
  "-target:jvm-1.8",
  "-encoding", "UTF-8",      // yes, this is 2 args
  "-unchecked",              // provide more info about type erasure
  "-deprecation",            // warn about deprecated usaage
  "-feature",
  "-language:existentials",
  "-language:higherKinds",
  "-language:implicitConversions",
  "-Xfatal-warnings",
  "-Xlint",
  "-Yno-adapted-args",
  "-Ywarn-dead-code",        // N.B. doesn't work well with the ??? hole
  "-Ywarn-numeric-widen",
  "-Ywarn-value-discard",    // warn when values aer discarded
  "-Xfuture",
  "-Ywarn-unused-import"     // 2.11 only
)
```
