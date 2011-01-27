# JavaScript #

## Basics ##

In JavaScript everything (except some primitive types) is an object, even a function. Every object can be treated as an associative array and can be accessed via `object.name = name` also known as dot notation (There are some problems with some reserved keyword though).

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

If you want more information the `istanceof` operator can help

	var toni = new Person("Toni", "Mustermann");
	var max  = new Employee("Max", "Muster", "Sales");
	alert(toni instanceof Person);	  // true
	alert(toni instanceof Employee);  // false
	alert(max  instanceof Person);	  // true
	alert(max  instanceof Employee);  // true

## Libraries/Frameworks ##

[zepto.js](https://github.com/madrobby/zepto)

> Zepto.js is a minimalist inlinable framework for mobile WebKit browsers, with a jQuery-like chaining syntax

[raphaël.js](https://github.com/DmitryBaranovskiy/raphael/)

> Raphaël is a small JavaScript library that should simplify your work with vector graphics on the web. (It) uses the SVG W3C Recommendation and VML as a base for creating graphics. This means every graphical object you create is also a DOM object, so you can attach JavaScript event handlers or modify them later. Raphaël’s goal is to provide an adapter that will make drawing vector art compatible cross-browser and easy.

[backbone.js](https://github.com/documentcloud/backbone)

> Backbone supplies structure to JavaScript-heavy applications by providing models key-value binding and custom events, collections with a rich API of enumerable functions, views with declarative event handling, and connects it all to your existing application over a RESTful JSON interface.

[QUnit](http://docs.jquery.com/Qunit)

> QUnit is a powerful, easy-to-use, JavaScript test suite. It's used by the jQuery project to test its code and plugins but is capable of testing any generic JavaScript code (and even capable of testing JavaScript code on the server-side).

[arbor.js]( http://arborjs.org)

> Arbor is a graph visualization library built with web workers and jQuery. Rather than trying to be an all-encompassing framework, arbor provides an efficient, force-directed layout algorithm plusabstractions for graph organization and screen refresh handling.


