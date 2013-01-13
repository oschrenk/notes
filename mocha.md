# Mocha #

- [Chai, Assertions API](http://chaijs.com/api/assert/)

> [Mocha](http://visionmedia.github.com/mocha/) is a feature-rich JavaScript test framework running on [node](http://nodejs.org/) and the browser, making asynchronous testing simple and fun. Mocha tests run serially, allowing for flexible and accurate reporting, while mapping uncaught exceptions to the correct test cases.

To install it globally

    $ npm install -g mocha

I prefer to install it per project though by adding it to the `package.json`:

	...
	"devDependencies": {
	    "mocha": "1.7.4"
  	},
  	...

Mocha supports multiple assertion styles (BDD, TDD). I prefer TDD and use [chai](http://chaijs.com/) for this. Also add it to your devDependencies

	"devDependencies": {
		"chai": "1.4.0",
		"mocha": "1.7.4"
	},

And create a Makefile

	test:
		@./node_modules/.bin/mocha -u tdd

	.PHONY: test

to instruct Mocha to use TDD style assertions. Mocha will run all test cases in `./test`. Example:

	var chai = require('chai');
	var assert = chai.assert;

	suite('acme', function() {

	  test('testCase1', function() {
	  	assert.isTrue(...);
	  });

	  test('testCase2', function() {
	  	assert.isFalse(...);
	  });

	});
