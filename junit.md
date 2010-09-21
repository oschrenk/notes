# JUnit 4

## Test Suites

	import org.junit.runner.RunWith;
	import org.junit.runners.Suite;
	import org.junit.runners.Suite.SuiteClasses;
	
	@RunWith(Suite.class)
	@SuiteClasses( { MySuite1.class, MySuite2.class, MySuite3.class })

	public class AllTests {
		// empty by choice
	}
