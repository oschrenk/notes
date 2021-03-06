# JavaScript #

- use [jsFiddle](http://jsfiddle.net/) as test playground

## Basics ##

In JavaScript everything (except some primitive types) is an object, even a function. Every object can be treated as an associative array and can be accessed via `object.name = name` also known as dot notation (There are some problems with some reserved keyword though).

### Bad Parts ###

Taken from [JavaScript: The Good Parts](http://www.youtube.com/watch?v=hQVTIJBZook) by Douglas Crockford

* Global Variables. No linker. Common global namespace where variables collide => Reliability, Security problems
* `+` adds and concatenates. Same as in Java, but as Java is strinlgy typed you could predict what happens, not with JavaScript.
* Semicolon Insertion. If compiler gets an error, it searches for a linefeed and inserts a semicolon, sometimes were its not intended
* typeof. Typeof array object -> not helpful, Typeof null is Object which is wrong
* with and eval. Eval most misused. If you want to use it step away and think.
* phony arrays. Normally linear arranged bucket. In JS arrays are like hashmaps. No dimensions.
* `==` and `!=`
* `false`, `null`, `undefined`, `NaN`

#### Transivity? ####

- look at [zero](http://zero.milosz.ca/) for an overview of type coercions

Type coercion on quality operator

	'' == '0' 			// false
	0 == '' 			// true
	0 == '0'			// true
	false = 'false'		// false
	false == '0'		// true
	false == undefined	// false
	false == null		// false
	null == undefined	// true
	" \t\r\n " == 0		// true

Always use triple equal operator, which does not use type coercion, and always used false in the cases above.

#### For..in ####

Does deep drilling. User has to filter own object's own properties

### Good Parts ###

* Lambda (Function as first class members)
* Dynamic Objects
* Loose Typing
* Object Literals

### Stuff ###

We have function scope not block scope

### Style ###

Rightness on brackets is neccesary! In the following example you would expect Javascript to return a new object with the `ok` member being `false`

	return
	{
		ok: false
	};

but it returns `undefined` because of semicolon inserion and its inherited c syntax. It becomes:

	return;
	{
		ok: false;
	}

with the block being ignored.

## Advanced ##

### Constructing objects ###

With the `new` operator functions are treated as constructors. The developer can reference the object from within the constructor via `this`.

	function Person(firstName, lastName) {
	  this.firstName = firstName;
	  this.lastName = lastName;
	  this.getName = function() {
	    return this.firstName + " " + this.lastName;
	  };
	}
	var testPerson = new Person("Max", "Muster");

### Prototyping ###

Defining methods from within the constructor has the disadvantage that every instance has its own instances of the function objects, so that memory is effectively lost when using multiple instances of an object.

You can _prototype_ an object. The prototype is then used as a fallback: If an object is asked for an attribute it doesn't have, the interpreter asks the prototype, repeating the process and asking for that prototype if the attribute can't be found until the end of the prototype chain.

	function Person(firstName, lastName) {
	  this.firstName = firstName;
	  this.lastName = lastName;
	}
	Person.prototype.getName = function() {
	  return this.firstName + " " + this.lastName;
	};
	var testPerson = new Person("Max", "Muster");

### Prototype Based Inheritance ###

You can use prototyping to create an object hierarchy.

	// Person is defined as above
	function Employee(firstName, lastName, department) {
	  Person.call(this, firstName, lastName);
	  this.department = department;
	};
	Employee.prototype = new Person();
	Employee.prototype.getName = function() {
	  return Person.prototype.getName.call(this) +
	    " (" + this.department + ")";
	};
	Employee.prototype.alertName = function() {
	  alert(this.getName());
	};
	var max = new Employee("Max", "Muster", "Sales");

As you may notice the constructor of `Person` is called and even without parameters. This makes little sense as we don't want to create an instance of `Person` at that point but rather establish the hierarchy. We can call the constructor without parameters as JavaScript will automatically set `undefined` for each unset variable.

Better practice is to replace

	Employee.prototype = new Person();

with

	var Temp = function() {};

	Temp.prototype = Person.prototype;
	Employee.prototype = new Temp();

This avoids calling the `Person` constructor.

You can call the super class constructor from the subclass by invoking the `call`` method.

	 Person.call(this, firstName, lastName);

The method is provided by the function object. With it you can define which object should be references when its calling `this` by injecting it as the first parameter.

### Object types ###

You can get general type information via `typeof`, but that would only return `Object` but the name of the constructor function.

If you want more information the `instanceof` operator can help

	var toni = new Person("Toni", "Mustermann");
	var max  = new Employee("Max", "Muster", "Sales");
	alert(toni instanceof Person);	  // true
	alert(toni instanceof Employee);  // false
	alert(max  instanceof Person);	  // true
	alert(max  instanceof Employee);  // true
