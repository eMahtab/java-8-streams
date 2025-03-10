# Java 8 Streams

## Creating stream from primitive type (int, long, double)

Java 8 comes with IntStream, LongStream, DoubleStream to work with stream of primitive types.

```java
import java.util.stream.IntStream;

public class Main {

    public static void main(String[] args) {
        int[] arr = {1,3,6,2,5,4,5};
        System.out.println(IntStream.of(arr).sum()); // 25
        System.out.println(IntStream.of(arr).average().getAsDouble()); // 3.7142857142857144
        System.out.println(IntStream.of(arr).min().getAsInt()); // 1
        System.out.println(IntStream.of(arr).max().getAsInt()); // 6
        IntStream.of(arr).sorted().forEach(num -> System.out.print(num+", ")); // 1, 2, 3, 4, 5, 5, 6, 
    }
}
```

## Stream interface mapToInt(), mapToLong() and mapToDouble() function which returns IntStream, LongStream, DoubleStream  

```java
IntStream mapToInt(ToIntFunction<? super T> mapper)

LongStream mapToLong(ToLongFunction<? super T> mapper)

DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper)
```

## Arrays.stream()
```
public static IntStream stream(int[] array)

public static LongStream stream(long[] array)

public static DoubleStream stream(double[] array)
```

## Converting stream of primitives to stream of wrapper types

By calling boxed() method on a primitve stream can be converted to stream of wrapper types.
```
IntStream's -> Stream<Integer> boxed()

LongStream's -> Stream<Long> boxed()

DoubleStream's -> Stream<Double> boxed()
```

### Sum stream elements
```java
import java.util.Arrays;
import java.util.stream.IntStream;
public class App {
	public static void main(String[] args) {
		int[] nums = {0,1,2,3,4,5,0,1,2,3,4,5};
		int sum1 = IntStream.of(nums).sum(); // Or IntStream.of(0,1,2,3,4,5,0,1,2,3,4,5).sum();
		int sum2 = Arrays.stream(nums).sum();
		System.out.println(sum1 + "," + (sum1==sum2));  // 30,true
	}
}
```
### range() and rangeClosed()
```java
public class App {
    public static void main(String[] args) {
       System.out.println("Sum :"+IntStream.range(1, 5).sum());  // Sum :10
       System.out.println("Sum :"+IntStream.rangeClosed(1, 5).sum());  // Sum :15
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

## Java 8 streams can't be reused  
After the terminal operation is performed, the stream pipeline is consumed and can't be used anymore.
  
```java
import java.util.stream.*;

public class Main{
  public static void main(String[] args) {
    int[] digits = {0, 1, 2, 3, 4 , 5, 6, 7, 8, 9};
    IntStream s = IntStream.of(digits);
    long n = s.count();
    System.out.println(s.findFirst()); // An exception will be thrown
  }
}
  
```  
## Output :
```
Exception in thread "main" java.lang.IllegalStateException: stream has already been operated upon or closed
    at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:229)
    at java.base/java.util.stream.IntPipeline.findFirst(IntPipeline.java:528)
    at Main.main(Main.java:8)  
  
```

```java
import java.util.stream.*;

public class Main{
  public static void main(String[] args) {
    int[] digits = {20, 1, 2, 3, 4 , 5, 6, 7, 8, 9};
    long n = IntStream.of(digits).count();
    System.out.println("Count : " + n);
    System.out.println(IntStream.of(digits).findFirst().getAsInt());
  }
} 
```  

## Output :
```
Count : 10
20  
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

### Passing Comparator :  
```java
import java.util.*;
import java.util.stream.*;

public class Main{
  public static void main(String[] args) {
    List<String> strings = Arrays.asList("Stream","Operations","on","Collections");
    strings.stream().min(Comparator.comparing(
                        (String s) -> s.length())
    ).ifPresent(System.out::println); // on
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

## Using flatMap() to flatten a stream of nested collection objects e.g. List<List<Integer>> listOfLists
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
public class App {
	public static void main(String[] args) {
		 // List of lists of integers
        List<List<Integer>> listOfLists = Arrays.asList(
            Arrays.asList(1, 2, 3),
            Arrays.asList(4, 5, 6),
            Arrays.asList(7, 8, 9)
        );

        // Using flatMap to flatten the list of lists into a single list
        List<Integer> flattenedList = 
        		listOfLists.stream()
                           .flatMap(list -> list.stream()) // Flatten the inner lists
                           .collect(Collectors.toList());

        // Print the flattened list
        System.out.println(flattenedList); //[1, 2, 3, 4, 5, 6, 7, 8, 9]
	}
}
```

```java
import java.util.List;
import java.util.Arrays;

public class Main {

    public static void main(String[] args) {
        List<List<Integer>> listOfLists = List.of(List.of(1, 2, 3), List.of(4, 5, 6));

        listOfLists.stream()
                .flatMap(list -> list.stream())
                .forEach(System.out::println);

        List<Integer[]> listOfArrays = List.of(new Integer[] { 1, 2, 3 }, new Integer[] { 4, 5, 6 });
        listOfArrays.stream()
                .flatMap(array -> Arrays.stream(array))
                .forEach(System.out::println);

    }
}
```

## Using flatMap() to flatten a stream of arrays e.g. List<Integer[]> listOfArrays

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Test {
	public static void main(String[] args) {
		 
        List<Integer[]> listOfArrays = Arrays.asList(
            new Integer[] {3, 2, 1},
            new Integer[] {6, 5, 4},
            new Integer[] {9, 8, 7}
        );

        // Using flatMap to flatten the list of arrays into a single list
        List<Integer> flattenedList = 
        		listOfArrays.stream()
                           .flatMap(array -> Stream.of(array)) // Flatten the array
                           .collect(Collectors.toList());

        // Print the flattened list
        System.out.println(flattenedList); //[3, 2, 1, 6, 5, 4, 9, 8, 7]
	}
}
```

## Using flatMap() to create stream of single(atomic) elements e.g. number, string etc.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
public class App {
	public static void main(String[] args) {
		List<String> sentences = Arrays.asList("Hello World", "Java Streams", "flatMap Example");
		List<String> words = sentences.stream()
		                              .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
		                              .collect(Collectors.toList());

		System.out.println(words); // [Hello, World, Java, Streams, flatMap, Example]
	}
}
```

## Using toMap() Collector to create a Map after processing the stream
```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

class Employee {
    private int id;
    private String name;
    private String department;
    public Employee(int id, String name, String department) {
        this.id = id;
        this.name = name;
        this.department = department;
    }
    public int getId() {
        return id;
    }
    public String getName() {
        return name;
    }
    public String getDepartment() {
        return department;
    }
    @Override
    public String toString() {
        return "{id=" + id + ", name='" + name + "', department='" + department + "'}";
    }
}

public class App {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee(101, "Alice", "HR"),
            new Employee(102, "Bob", "Finance"),
            new Employee(103, "Charlie", "IT"),
            new Employee(104, "David", "HR")
        );

        // Convert the list to a map with employee ID as key and employee name as value
        Map<Integer, String> employeeMap = employees.stream()
            .collect(Collectors.toMap((emp) -> emp.getId(), (emp) -> emp.getName()));

        System.out.println("Employee Map: " + employeeMap);
        // Employee Map: {101=Alice, 102=Bob, 103=Charlie, 104=David}

        // Another use case: Creating a map with employee ID as key and the whole Employee object as value
        Map<Integer, Employee> employeeDetailMap = employees.stream()
            .collect(Collectors.toMap((emp) -> emp.getId(), (emp) -> emp));

        System.out.println("Employee Detail Map: " + employeeDetailMap);
        // Employee Detail Map: {101={id=101, name='Alice', department='HR'}, 102={id=102, name='Bob', department='Finance'}, 103={id=103, name='Charlie', department='IT'}, 104={id=104, name='David', department='HR'}}
    }
}

```

## Sorting the stream elements with custom comparator
```java
import java.util.Arrays;
import java.util.List;

public class Test {
	public static void main(String[] args) {
        List<String> words = Arrays.asList("banana", "cherry", "apple", "dates");
        List<String> sorted = words.stream()
        		                   .sorted((s1, s2) -> s2.compareTo(s1))
        		                   .toList();
        System.out.println(sorted); // [dates, cherry, banana, apple]
	}
}
```
