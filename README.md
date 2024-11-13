# Java 8 Streams

### Sum stream elements
```java
import java.util.Arrays;
import java.util.stream.IntStream;
public class App {
	public static void main(String[] args) {
		int[] nums = {0,1,2,3,4,5,0,1,2,3,4,5};
		int sum1 = IntStream.of(nums).sum();
		int sum2 = Arrays.stream(nums).sum();
		System.out.println(sum1 + "," + (sum1==sum2));  // 30,true
	}
}
```
### Removing duplicates
```java
import java.util.stream.IntStream;
public class App {
	public static void main(String[] args) {
		int[] nums = {0,1,2,3,4,5,0,1,2,3,4,5};
		IntStream.of(nums)
		         .distinct()
		         .forEach((num) -> System.out.print(num+", ")); // 0, 1, 2, 3, 4, 5,
	}
}
```
