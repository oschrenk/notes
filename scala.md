# Scala ##

	brew install scala

- [sbt](http://www.scala-sbt.org/) build tool for Scala and Java projects
- [Scala IDE](http://scala-ide.org/) Eclipse Plugin

## Language ##

- statically typed
- it is functional: functions are values
- it is object oriented: every value is an object
- compiles to bytecode for the JVM and CLR

### Conventions ###

- Methods include parentheses if it has side effects

### Companion objects

- provide means to associate functionality with a class without associating it to a any instance
- has same name as its associated class
- must be defined in the same file

## Functional Programming ##

- Functional languages treat functions a _first-class values_
- Anonymous Functions are Syntactic Sugar.
- *function application syntax* an object can be "called" like a function if it has the `apply` method

### Tail recursion ###

If a function calls itself as its last action, the function's stack frame can be reused. This is called _tail recursion_. => Tail recursive functions are iterative processes. It is the functional form of a loop and executes as efficiently as a loop and can execute in constance space.

## Style ##

- A typical way is to first define a function which computes one iteration step
- It's good functional programming style to split up a task into many small functions
- Always take care of the boundary cases first when dealing with collections

- the ability to choose different implementations of the data without affecting clients is called _data abstraction_. It is a cornerstone of software engineering.

## Compiler ##

### Evaluation Strategy ###

- Call-by-name `def power(x: Double, y: Int): Double = ...` Evaluation from left to right)
- Call-by-value `def power(x: Double, y => Int): Double = ...` Unused parameter not evaluated

## Implementation ##

- Scala uses implicit returns but recursive functions need explicit return type

### Functions ###

Example for a simple recursive function

	def sqrtIter (guess: Double, x: Double): Double =
  		if (isGoodEnough(guess, x)) guess
  		else sqrtIter(improve(guess, x), x)

	...

Example for a a higher order function, meaning that it the function takes another function as an argument.

	def sum (f: Int => Int, a: Int, b: Int): Int =
	if (a > b) 0
	else f(a) + sum (f, a + 1, b)

Example for a function that returns a function

	def sum (f: Int => Int): (Int, Int) => Int ) {
		def sumF(a: Int, b: Int) =
			if (a > b) 0
			else f(a) + sumF(a + 1, b)
		sumF
	}

- functions in data abstractions (_classes_) are called _methods_

### Types ###

The `Option` type is defined as

	trait Option[+A]
	case class Some[+A](value A) extends Option[A]
	object None extends Option[Nothing]

### Object Definitions ###

A _singleton object_, for which only one instance can be (or need to be) created use `object`.

	object Empty extends IntSet {
		...
	}

No other `Empty` instances can be (or need to be) created. Singleton objects are values, so `Empty` evaluates to itself.

### Classes ###

	class Rational (x: Int, y: Int) {
		def numer = x
		def denom = y
	}

This introduces two entities

1. A new _type_, named `Rational`
2. A new _constructor_ `Rational` to create elements of this type

The elements of a a class type are _objects_, for example

	new Rational (1, 2)

- functions in data abstractions (_classes_) are called _methods_

- `override toString` is used to print
- `private` modifier to hode members and methods
- `this` refers to the object on which current method is executed

To add another constructor, use `this` in function position

	class Rational (x: Int, y: Int) {
		def this(x: Int) ) this(x, 1)
		...
	}

Auxiliary constructor

	class Poly(terms0: Map[Int, Double]) {
    	def this(bindings: (Int, Double)*) = this(bindings.toMap)
    }

The `*` is used for repeated parameters.

#### Hierarchies ####

_Abstract classes_ can contain members missing an implementation. No instances of an abstract class can be created with `new` operator.

	abstract class IntSet {
		def incl (x: Int): IntSet
		def contains(x: Int): Boolean
	}

Every class that `extend`s the class `IntSet` conform to the type `IntSet`

	class Empty extends IntSet {
		...
	}
	class NonEmpty(elem: Int, l: IntSet, r: IntSet) extends IntSet {
		...
	}

- `IntSet` is called _superclass_ of `Empty` and `NonEmpty`
- `Empty` and `NonEmpty` are _subclasses_ of `IntSet`
- If no superclass is given, standard class `java.lang.Object` is assumed
- All direct or indirect superclasses of `C` are called base _classes_
- it is possible to _redefine_ an existing, non abstract defintion by using `override`

#### Traits ####

Traits resemble interfaces in Java, but are more powerful because they can contain fields and concrete methods. On the other hand traits cannot have (value) parameters, only classes can.

	trait Planar {
		def height: Int
		def width: Int
		def surface: Int = height * width
	}

Classes, objects and traits can inherit at most one class but arbitrary many traits.

	class Square extends Shape with Planar with Movable

Cannot have a constructor

*Sealed traits* allow to enumerate all the possible classes that extend a trait and have the compiler warn if a pattern matching expression is missing a case. All subtypes have to be defined in the same file. The majority of cases should use the sealed trait pattern.

#### Case class ####

1. You can do pattern matching on it,
2. You can construct instances of these classes without using the new keyword. It adds a factory method with the name of the class. This means you can write say, `Var("x")` to construct a Var object instead of the slightly longer new `Var("x")`
3. All constructor arguments are accessible from outside using automatically generated accessor functions. All arguments in the parameter list of a case class implicitly get a val prefix, so they are maintained as fields.
4. The `toString` method is automatically redefined to print the name of the case class and all its arguments,
5. The `equals` method is automatically redefined to compare two instances of the same case class structurally rather than by identity.
6. The `hashCode` method is automatically redefined to use the hashCodes of constructor arguments.

Most of the time you declare a class as a case class because of point 1, i.e. to be able to do pattern matching on its instances. But of course you can also do it because of one of the other points.

#### Case object ####

- if you have a class without constructor arguments, you can define it as `case object`

#### Companion Object ####

A companion object is an object with the same name as a class or trait and is defined in the same source file as the associated file or trait.

A companion object differs from other objects as it has access rights to the class/trait that other objects do not. In particular it can access methods and fields that are private in the class/trait.

#### Programs ####

Each application contains an object with a `main` method.

	object Hello {
		def main (args: Array[String]) = println("hello world")
	}

Once this program is compiled, you can start it from the command line.

	> scala Hello

### Data ###

Guarding against invalid values

`require` to enforce precondition on the caller of a function

	class Rational (x: Int, y: Int) {
		// throws IllegalArgumentException
		require (y != 0, "denominator must be nonzero")
		def numer = x
		def denom = y
	}

`assert` is used to check the code of the function itself. It's not the callers fault

	val x = sqrt(y)
	// throw AssertionError
	assert(x >= 0)

## Collections ##

Scala's immutable collections are:

- _easy to use_: few steps to do the job
- _concise_: one word replaces a whole loop
- _safe_: type checker is really good at catching errors
- _fast_: collection ops are tuned, can be parallelized
- _universal_ once vocabulary to work on all kinds of collections

### Lists ###

#### Map Function ####

Transforms each element of a list and then returns the list of results. There are various ways of defining this:

By doing pattern matching:

	def squareList (xs: List[Int]): List[Int] = xs match {
		case Nil 		=> Nil
		case y :: ys	=> y * y :: ys
	}

By using the `map` function

	def squareList (xs: List[Int]): List[Int] = xs match {
		xs map (x => x * x)
	}

#### Filter Function ####

Select all elements from a list satisfying a given condition. There are various ways of defining this:

By doing pattern matching:

	def posElems (xs: List[Int]): List[Int] = xs match {
		case Nil		=> 	xs
		case y :: ys 	=>	if (y > 0) y :: posElems(ys) else posElems(ys)
	}

By using the `filter` function

	def posElems (xs: List[Int]): List[Int] =
		xs filter (x = x > 0)

There are other helpful filter functions

- `xs filterNot p` filter elements of `xs` that do not satisfy predicate `p`
- `xs partition p` Returns the pair `(xs filter p, xs filterNot p)` in a single traversal of the list `xs`
- `xs takeWhile p` Longest prefix of list `xs` consisting of elements satisfying predicate `p`
- `xs dropWhile p` Remainder if the list `xs` after any leading elements satisfying `p` have been removed
- `xs span p` Returns the pair `(xs takeWhile p, xs dropWhile p)` in a single traversal of the list `xs`

#### Reduction ####

Combine elements of a list. There are various ways of defining this:

By doing pattern matching:

	def sum(xs: List[Int]): Int = xs match {
		case Nil		=> 0
		case y :: ys 	=> y + sum(ys)
	}

By using the `reduceLeft` function

	def sum(xs: List[Int]): Int =
		(0 :: xs) reduceLeft ((x,y) => x + y)

Or using the above function in a shorthand notation

	def sum(xs: List[Int]): Int =
		(0 :: xs) reduceLeft (_ + _)

with `_` representing a new parameter from left to right

`reduceLeft` can be only applied to non empty lists! A more general function called `foldLeft` takes an **accumulator**, `z`, as an additional parameter.

	def sum(xs: List[Int]): Int =
		(xs foldLeft 0) (_ + _)

Dual functions `foldRight` and `reduceRight` that produce trees which lean to the right exist. For associative and commutative operators, `foldLeft` and `foldRight` are equivalent (but may differ in efficiency). Sometimes, only one of them is appropriate. For example:

	def concat[T](xs: List[T], ys: List[T]): List[T] =
		(xs foldRight ys) (_ :: _)

### Vector ###

Lists are _linear_. Access to first element is faster than access to the middle or end. Scala offers other implementations for sequences, such as the `Vector`.

Has all the operations as lists, with the exeptions of `::`. Instead of `x :: xs`, there is

- `x +: xs` Create new vector with leading element `x`, followed by `xs`
- `x :+ xs` Create new vector with trailing element `x`, preceded by `xs`

### Range ###

Represents a sequence of evenly spaced integers. Three operators

- `to` inclusive
- `until` exclusive
- `by` determine step value

Examples

	val r: Range = 1 until 5 	// 1,2,3,4
	val s: Range = 1 to 5		// 1,2,3,4,5
	1 to 10 by 3 				// 1,4,7,10
	6 to 1 by -2				// 6,4,2

Ranges are represented as single obj.ects with only three fields: lower bound, upper bound, step value

### Functions on Sequences ###

	val xs = Array (1, 2, 3, 44)
	xs map (x => x  * 2)

	val s = "Hello World"
	s filter (c => c.isUpper)		// res1: String HW

	s exists (c => c.isUpper)		// res2: Boolean true
	s forall (c => c.isUpper)		// res3: Boolean false

	val pairs = List(1,2,3) zip s // pairs: List[(Int, Char)] = List (1,H, (2,e), (3,l))
	pairs.unzip					// res4: (List[Int), List[Char]) = (List(1,2,3), List(H,e,l))
	s flatMap (c => List('.', c)) // res5: String = .H.e.l.l.o. .W.o.r.l.d

Example: List all combinations of numbers `x` and `y` where `x` is drawn from `1..M` and `y` from `1..N`

	(1 to M) flatMap (x => (1..N) map (y => (x,y)))

Example: Scalar Product

	def scalarProduct (xs: Vector[Double], ys: Vecotr[Double]): Double =
		(xs zip ys) map (xy._1 * xy._2 ).sum

or using **pattern matching function value**

	def scalarProduct (xs: Vector[Double], ys: Vecotr[Double]): Double =
		(xs zip ys) map {case (x,y) => x * y }.sum

### For ... yield ###

Functions such as `map`, `flatMap` or `filter` provide powerful constructs for manipulating lists but can be quite cumbersome to use.

Example:

	case class Person(name: String, age Int)

To obtain the names of persons over 20 years old, you can write:

	persons filter (p => p.age > 20) map (p => p.name)

but you can also use the `for` expression

	for (p <- persons if p.age > 20) yield p.name

The for-expression is similar to loops in imperative languages, except that it build a list of the results of all iterations.

Example: Given a positive integer `n`, find all the pairs of positive integers `(i,j)` so that `1 <= j < i < n`, and `i+j` prime.

	for {
		i <- 1 until n
		j <- 1 until i
		if isPrime(i+ j)
	} yield (i, j)

### Sets ###

1. Sets are unordered; elements of a set do not have a predefined order i  which they appear
2. `sets` do not have duplicate elements
3. The fundamental operation on sets is `contains`

### Maps ###

A map of type `Map[Key, Value]` is a data structure that associates keys of type `Key` with values of type `Value`

	val romanNumerals = Map("I" -> 1, "V" -> 5. "X" -> 10)

The expression `map get key` returns

- `None` if `map` does not contain the given key,
- `Some(x)` if `map` associates the given `key` with value `x`

Since `Option` is defined as case classes, they can be decomposed using pattern matching.

	def showCapital(country: String) = capitalOfCountry.get(country) match {
		case Some(capital) => capital
		case None => "mising data"
	}

By default maps are **partial functions**. Applying a map to a key value in `map(key)` could lead to an exception, if the key was not stored in the map. The operation `withDefaultValue` turns a map into a total function

	val cap1 = capitalOfCountry withDefaultValue "<unknown>"#
	cap1("Andorra") 	// "<unknown>"

## Comparison to Java ##

### Syntax ###

	// Java/C#
	public static void main(String[] args) //Body

	// Scala
	def main(args: Array[String]) = //Body

- the concept of static methods (or variables or classes) doesn't exist in Scala. In Scala everything is an object.
- Scala has an `object` construct with which we can declare singletons. In other word our main method is an instance method on a singleton object that is automatically instantiated for us.
- no return type. In fact there is a return type, Unit which is similar to void, but it’s inferred by the compiler. Should we want to we can explicitly specify the return type by putting a colon and the type after the parameters
- default access level is public.

### Concurrency ###

> Java's threading model is built around shared memory and locking, a model that is often difficult to reason about

Scala p. 53

> An arguably safer alternative is message pasing architecture, such as the "actors" approach. [...] Actors are concurrency abstractions that can be implemented on top of threads. They communicate by sending messages to each other.

	recipient ! msg

The send operation is denoted by `!` and is executed asynchronously. An actor handles messages that have arrived in its mailbox via a `receive` block.

	receive {
		case Msg1 => ...
		case Msg2 => ...
	}

