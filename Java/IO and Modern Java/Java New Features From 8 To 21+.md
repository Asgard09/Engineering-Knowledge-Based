# 1. Java 8
## 1.1. Functional Interfaces
An interface with exactly one abstract method. Annotated with `@FunctionalInterface`:

|Interface|Method|Use Case|
|---|---|---|
|`Function<T, R>`|`R apply(T t)`|Transform: T → R|
|`Predicate<T>`|`boolean test(T t)`|Filter: T → boolean|
|`Consumer<T>`|`void accept(T t)`|Side-effect: T → void|
|`Supplier<T>`|`T get()`|Factory: () → T|
|`UnaryOperator<T>`|`T apply(T t)`|Transform: T → T|
|`BiFunction<T, U, R>`|`R apply(T t, U u)`|Transform: (T, U) → R|
## 1.2. Stream API
```Java
List<String> names = people.stream()
    .filter(p -> p.getAge() > 18)           // filter
    .sorted(Comparator.comparing(Person::getName))  // sort
    .map(Person::getName)                    // transform
    .distinct()                              // remove duplicates
    .limit(10)                               // take first 10
    .collect(Collectors.toList());           // terminal operation

// Reduction
int totalAge = people.stream()
    .mapToInt(Person::getAge)
    .sum();

// Grouping
Map<String, List<Person>> byCity = people.stream()
    .collect(Collectors.groupingBy(Person::getCity));

// Parallel stream (use with caution)
long count = list.parallelStream()
    .filter(s -> s.length() > 5)
    .count();
```