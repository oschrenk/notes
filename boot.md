# Boot

On the first look `boot` looks like a `lein` replacement, but going deeper it allows for a nice aabstraction, an abstraction written in clojure no less, to build your app. `boot` defines _tasks_, units of logic that communicate through _filesets_ .

## What is a task?

Tasks are

- named operations
- that take command line options
- that dispatch other programs

```shell
mkdir boot-walkthrough
cd boot-walkthrough
touch build.boot
```

And fill it with

```clojure
(deftask fire
  "Announces that something is on fire"
  [t thing     THING str  "The thing that's on fire"
   p pluralize       bool "Whether to pluralize"]
  (let [verb (if pluralize "are" "is")]
    (println "My" thing verb "on fire!")))
```
