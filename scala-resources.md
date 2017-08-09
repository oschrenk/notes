# Scala Resources

- [fommil/netlib-java](https://github.com/fommil/netlib-java) Mission-critical components for linear algebra systems, with Fortran performance.
- [scalanlp/breeze](https://github.com/scalanlp/breeze) numerical processing library
- [scopt/scopt](https://github.com/scopt/scopt) simple scala command line options parsing
- [sameersingh/scalaplot](https://github.com/sameersingh/scalaplot) plot graphs with gnuplot as backend
- [better-files](https://github.com/pathikrit/better-files) Dependency free Scala library for I/O

## Parsers

- [FastParse](http://lihaoyi.github.io/fastparse/?) Parser combinator library
- [Parboiled2](https://github.com/sirthias/parboiled2) Macro-based PEG parser generator

## Learning

https://www.reddit.com/r/ScalaConferenceVideos/

* If you want to learn Scala, I'd suggest:
  - Install IntelliJ + Scala Plugin
  - Don't do the Coursera courses yet.
  - Don't do the "red book"  Functional Programming in Scala yet.
  - Do: http://underscore.io/books/
    - Essential Scala
    - Essential Play
    - Essential Slick
  - Do Scala for the Impatient: https://www.amazon.com/Scala-Impatient-Cay-S-Horstmann/dp/0321774094
  - The Neophyte's Guide to Scala http://danielwestheide.com/scala/neophytes.html
  - Write Scala code for a few weeks/months.
  - Do pair programming with experienced Scala devs.
  - Install and configure scalafmt http://scalameta.org/scalafmt/
    - Just focus on the code, not the style.
  - Go to gitter channels, people are really nice there.
    - https://gitter.im/scala/scala

* Improving Scala/FP/Reactive Programming
  - Akka
    - Akka in Action https://www.manning.com/books/akka-in-action
    - Effective Akka http://shop.oreilly.com/product/0636920028789.do
    - More: http://doc.akka.io/docs/akka/current/scala/additional/books.html
  - Read about SBT (but not too much)
    - Keep your build.sbt simple, the less, the better.
  - Install ammonite
  - Stop using IntelliJ compiler, it'll eventually lie to you. Always compile on SBT.
  - Start reading the "red book" https://www.manning.com/books/functional-programming-in-scala
  - Don't throw Exceptions, use ADTs, Either
  - Do Advanced Scala with Cats (underscore)
  - Keep reading the "red book"
  - Keep and eye on: Functional Programming in Scala for Mortals
    - https://leanpub.com/fp-scala-mortals
  - Functional and Reactive Domain Modeling
    - https://www.manning.com/books/functional-and-reactive-domain-modeling
  - Keep reading the "red book"
  - Read more blogs, twitter about Scala.
    - From time to time, they will fight, just ignore them.
  - read.andThen(practice).recoverWith(read)

  - If you have plenty of time do the Coursera Courses (not sure if it'd worth it)
* Read more about FP, Cats/Scalaz, Akka Streams, Monix, CQRS, and all the cool libs/patterns out there.

Update:
2017-08-01: Added Scala for the Impatient https://twitter.com/d1egoaz/status/892564253751758848
