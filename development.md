# Software Development #

## XML vs. JSON ##

Over the time I learned to really dislike the verbosity of xml. There are nice tools out there to handle the data and the complexity but they all seem to try to solve a problem by throwing only more stuff at it.

Xml normally undergoes these transitions:
 
	data space > document space > data space

Other formats such as JSON and its spiritual ancestors like yaml on live in `data space`. I especially like JSON as a JSON object is already a JavaScript object and an Ruby (1.9+) object. Isn't that neat!

## Modularity and Testability ##

### Dependency Injection ###

_Dependency Injection_ <sup class="footnote">[1](#fn1)</sup> or _DI_ in short is the process of defining dependencies of external objects.

Three types of DI:

*   _Type 1_ or _Interface Injection_: an interface is added to the
    class
*   _Type 2_ or _Setter Injection_: _Setter_-methods to inject a
    dependency
*   _Type 3_ or _Constructor Injection_ the constructor of the module
    contains the dependencies

## API/Framework Design ##

### Fluent Interfaces ###

Fluent interfaces <sup class="footnote">[2](#fn2)</sup> only make sense if a developer uses them but not if he has to write classes fulfilling the contract.

## Sources ##

<p class="footnote" id="fn1"><sup>1</sup> <a href="http://martinfowler.com/articles/injection.html#InversionOfControl">Inversion of Control</a> (Martin Fowler, 04-01-23)</p>

<p class="footnote" id="fn2"><sup>2</sup> <a href="http://martinfowler.com/bliki/FluentInterface.html">FluentInterface</a> (Martin Fowler, 05-12-20)</p>
