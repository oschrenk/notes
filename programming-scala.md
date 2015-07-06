# Programming in Scala

Writing methods

1. Identify input and output.
2. Prepare test cases
3. Write the declaration
4. Run the code
5. Write the body
5.1 Consider the the result type.
5.2 Consider the input type
6. Run the code, again

Translate data

Data normally describes two relationships:

1. _has-a_ "A _has_ a B _and_ a C" eg. a Cat has a colour and a favorite food
   We can model this with a `case class` or a `trait`
   ```
   case class A(b: B, c: C)

   trait A {
     def b: B
     def c: C
   }
   ```

2. _is-a_ "A _is_ a B _or_ C" eg. a `Feline` is a `Cat` or `Lion`. We can model this using the sealed tait/final case class pattern
   ```
   sealed trait A
   final case class B() extends A
   final case class C() extends B
   ```

There are three ways to implement structural recursion

1. polymorphism
2. pattern matching in the base trait
3. pattern matching in an external object

Recursive Algebraic Data Types Pattern

When defining recursive algebraic data types, there must be at least two cases: one that is recursive, and one that is not.

```
sealed trait RecursiveExample
final case class RecursiveCase(recursion: RecursiveExample) extends RecursiveExample
final case object BaseCase extends RecursiveExample
```

Tail recursion

You may be concerned that recursive calls will consume excessive stack space. Scala can apply an optimisation, called _tail recursion_ to stop them consuming stack space. A tail call is a methidcall where the caller immediately returns the value eg.

```
def method1: Int = 1
def tailCall = method1
```

Due to limitations of the JVM, Scala can only optimise tail calls where the caller calls itself. You can use the `@tailrec` annotation to instruct the compiler to check tail recursion.

Any non-tail recursion function can be transformed into a tail recursive version by adding an accumulator. This transforms stack allocation into heap allocation, which sometimes is a win, sometimes its not.

