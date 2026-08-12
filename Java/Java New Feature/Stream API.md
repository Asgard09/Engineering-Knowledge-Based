# 1. Definition
Stream API was added in java 8 to ease the task of performing bulk operation. It provide two keys of abstraction:
- stream: represents a finite or infinite sequence of data elements. 
- stream pipeline: multistage computation on these elements.
The data of element can be object or primitive (int, long, double).
# 2. Key concepts and features
| Feature          | Description                                                                                                                                                               |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Source           | Where the stream data comes from (Collection, array, I/O, generator).                                                                                                     |
| Intermediate Ops | Operations returning new stream (e.g., filter(), map(), sorted()); lazy evaluated (nothing happen until call a termination ops)                                           |
| Terminal Ops     | Operations producing a result or a side-effect (e.g., collect(), forEach(), reduce(), count()). After a terminal operation, the stream is closed and can’t be used again. |
| Pipelining       | Linking operations together to process data step by step.                                                                                                                 |
| Immutability     | Stream operations do not alter the source data.                                                                                                                           |
| Short-Circuiting | Some operations terminate processing early (e.g., anyMatch(), findFirst()).                                                                                               |
# 3. Creating Streams
```java
// From a collection
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream1 = list.stream();

// From an array
Stream<Integer> stream2 = Stream.of(1, 2, 3, 4);

// From a range 
IntStream intStream = IntStream.range(1, 10);
```

# 4. Common stream operations
## 4.1. Filtering and Processing
```java
list.stream()
	.filter(item -> item.startsWith("J"))
	.forEach(System.out::println);
```
## 4.2. Mapping and Transformation
```java
list.stream()
	.map(String::toUpperCase)
	.forEach(System.out::println);
```
## 4.3. Sorting
```java
list.stream()
	.sorted()
	.forEach(System.out::println());
```
## 4.4. Collecting Result
```java
list.stream()
	.map(String::toUpperCase)
	.collect(Collectors.toList());
```
## 4.5. Reducing
```java
int sum = Steam.of(1,2,3)
			.reduce(0, Integer::sum)
```
