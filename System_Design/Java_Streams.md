### Definition:
1. A Stream in Java represents a sequence of elements supporting sequential and parallel aggregate operations. It allows functional-style operations on collections of elements, such as map, filter, reduce, etc.
1. It does not store data; instead, it operates on data from a source (like a Collection, List, Set, Map, or array), processes it, and produces a result.
1. We can also consider stream as a pieline through which elements are passed and openrations are performed on that.
1. Useful when dealing with bulk processing(large data size)(can deal with parellel processing).
1. Introduced in java 8

### Steps:

### Intermediate operations:

### Sequential processing of streams:
```java
List<Integer> salary = Arrays.asList(3,2,1,6,4,4);
        Stream<Integer> salaryStream = salary.stream();

        salaryStream.filter((Integer val) -> val>2)
                .peek((Integer val) -> System.out.println("filtering: "+val))
                .map((Integer val) -> val+10)
                .peek((Integer val) -> System.out.println("mapping: "+val))
                .sorted()
                .peek((Integer val) -> System.out.println("sorted: "+val))
                .collect(Collectors.toList());
```
### Parellel Stream:
1. helps to perform operations on streams concurrently, taking advantage of multicode CPU.
1. prallelStream() method is used instead of stream() method.
1. Internally used forkJoinPool to divide task into subtask, and doing them parellely.
