# JUnit-Tests


=== "UmrechnungTimeZeit.java"
	```java
	public class UmrechnungTimeZeit {
		
		public String convert(String time)
		{
			return "";
		}

	}
	```

`File --> New --> JUnit Test Case`

=== "TestUmrechnungTimeZeit.java"
	```java
	import static org.junit.jupiter.api.Assertions.*;
	import org.junit.jupiter.api.Test;

	class TestUmrechnungTimeZeit {

		@Test
		void test() {
			fail("Not yet implemented");
		}

	}
	```

=== "module-info.java"
	```java
		requires org.junit.jupiter.api;
	```

Die Methode `test()` in `TestUmrechnungTimeZeit.java` umbenennen in `testConvert1amTo1(String time)`.