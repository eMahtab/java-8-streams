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

### Filter elements
```java
import java.util.stream.IntStream;
public class App {
	public static void main(String[] args) {
		int[] nums = {0,1,2,3,4,5,0,1,2,3,4,5};
		IntStream.of(nums)
		         .distinct()
		         .filter((num) -> num % 2 == 0)
		         .forEach((num) -> System.out.print(num+", ")); // 0, 2, 4, 
	}
}
```
### Using map() and collecting the proccessed list
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
public class App {
	public static void main(String[] args) {
		List<String> words = Arrays.asList("Java", "8", "Streams");
		List<Integer> wordLengths = 
		    words.stream()
		         .map(str -> str.length())
		         .collect(Collectors.toList());
		System.out.println(wordLengths); // [4,1,7]
	}
}
```

### Stream of Objects 
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
public class App {
	public static void main(String[] args) {
		List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
		List<Integer> twoEvenSquares = 
		    numbers.stream()
		           .filter(n -> {
		                    System.out.println("filtering " + n); 
		                    return n % 2 == 0;
		                  })
		           .map(n -> {
		                    System.out.println("mapping " + n);
		                    return n * n;
		                  })
		           .limit(2)
		           .collect(Collectors.toList());
		System.out.println("Output :"+twoEvenSquares);
	}
}
```
### Output
```
filtering 1
filtering 2
mapping 2
filtering 3
filtering 4
mapping 4
Output :[4, 16]
```

