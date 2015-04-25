# Clojure

- dynamically typed

## Constituents

```
2r101
```

## `seq`

- a sequence of things
- a common interface

## `require`/`use`

- needs to have quoted symbol/expression/form, otherwise Clojure will try to resolve it

- `use` is similar to require. Makes everything available in your namespace. Watch out for name clashes

## `ns`

- can accept unquoted symbols, because it's a macro

## Metadata

- introduced by `^`
- `^:dynamic` often used by libraries for default configuration
- `binding`

## Bindings


### local/`let`

- can create local bindings which live in local lexical scope

```clojure
(let [x 42] (str "I got " x))
```

is translated to

```clojure
((fn [x] (str "I got " x)) 42)
```

## Functions

```
; named functions allow recursion
```


- `partial` takes a function `f` and fewer arguments than f itself, and returns a function that takes a variable number of additional args.

- `apply` is used to apply an operator to its operands. It unwraps a list to form an argument list.

- `map` applies a function f to each element

### Higher order functions

```clojure
(defn around [f x]
  (println (str "I'm about to apply" f " to " x))
  (f x))
```


### Multiple arities

(defn fib
  ([n]
  ...)
  ([x n]
  ))

### Recursion

`recur` will ensure *Tail Call Optimization*

### Loops

- `loop` works like local recursion without function names
- `for` works like list comprehension

### Lazyness

## Project definition

`project.clj`

- profiles define different env
  + when running from repl, `dev` profile is run by default

## Testing

### Midje

When midje ecnounters symbols with a dot, it treats it as a placeholder, enabling mocked behavior

```
..movie..
```

- `roughly` for double tolerance testing

## Threading primitives

```
(doc promise)
(doc future)
```

- `future`s return automatically, are run in a dedicated thread pool

- `atom`
- `agent`
- `ref` implements software transactional memory

## Protocols

- allows for runtime polymorphism
- manipulates dipatch table

## OO

- defmethod multimethod dispatch

## Types

- `core.typed`
- `schema.core`

## Macros

- use with extreme care

use `~` if you want to replace, sort of like `$` in a shell script




---------

- read again about tail call Optimization
- root-binding???
- Symbols are pointers to something else, evaluates to variables, which in turn evaluate to values
- a `form` is everything that reader understands
- `when` is usually used for side effects
- read more about `thunk`
- hypirion understand persistent vecto
- difference between top-down tdd and bdd
- look up transactional memory
- Book: Structure and Interpretation of Computer Programs
- virtual dispatch
- inheritance
- polymorphism
- what means turing complete

- destructuring

```
let [[head & tail] coll]]
```
