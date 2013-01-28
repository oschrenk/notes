# JUnit 4

- [nosql-unit](https://github.com/lordofthejars/nosql-unit) NoSQL Unit is a JUnit extension that helps you write NoSQL unit tests.
- [DbUnit](http://www.dbunit.org/). Open Source. Free. A JUnit extension targeted at database-driven projects that, among other things, puts your database into a known state between test runs
- [XmlUnit](http://xmlunit.sourceforge.net/) Open Source. Free. A Junit extensions that allows assertions about differences between two pieces of XML.

## Test Suites

	import org.junit.runner.RunWith;
	import org.junit.runners.Suite;
	import org.junit.runners.Suite.SuiteClasses;

	@RunWith(Suite.class)
	@SuiteClasses( { MySuite1.class, MySuite2.class, MySuite3.class })

	public class AllTests {
		// empty by choice
	}

