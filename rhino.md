# Rhino #

[Rhino](https://developer.mozilla.org/en-US/docs/Rhino) is an open-source implementation of JavaScript written entirely in Java. It is typically embedded into Java applications to provide scripting to end users. It is embedded in J2SE 6 as the default Java scripting engine.

## org.mozilla.javascript.EcmaError: TypeError: ? is not a function, it is object ##

The error was a wrong import (using the wrong package) in the javascript file

	Target = Packages.com.srhub.rules.js.DynamicTarget;