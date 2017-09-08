# lambda-practices

**函数接口**

```
      +----------+
T --> | Function | --> R
      +----------+
```
```
      +----------+
-X->  | Supplier | --> T
      +----------+
```
```
      +----------+
T --> | Consumer | -X->
      +----------+
```
```
      +-----------+
T --> | Predicate | --> boolean
      +-----------+
```
```
          +------------+
(T,U) --> | BiConsumer | -X->
          +------------+
```
```
          +------------+
(T,U) --> | BiFunction | --> R
          +------------+
```
```
          +----------------+
(T,T) --> | BinaryOperator | --> T
          +----------------+
```
```
          +-----------+
(T,U) --> | Predicate | --> boolean
          +-----------+
```
```
      +-----------------+
-X->  | BooleanSupplier | --> boolean
      +-----------------+
```
```
                     +----------------------+
(double, double) --> | DoubleBinaryOperator | --> double
                     +----------------------+
```
```
           +----------------+
double --> | DoubleConsumer | -X->
           +----------------+
```
```
           +----------------+
double --> | DoubleFunction | --> R
           +----------------+
```
```
           +-----------------+
double --> | DoublePredicate | --> boolean
           +-----------------+
```
```
     +----------------+
-X-> | DoubleSupplier | --> double
     +----------------+
```
```
           +---------------------+
double --> | DoubleToIntFunction | --> int
           +---------------------+
```
```
           +---------------------+
double --> | DoubleToIntFunction | --> long
           +---------------------+
```
```
           +---------------------+
double --> | DoubleUnaryOperator | --> double
           +---------------------+
```
```
               +-------------------+
(T,double) --> | ObjDoubleConsumer | -X->
               +-------------------+
```
```
          +-------------------+
(T,U) --> | ToDoubleBiFunction | --> double
          +-------------------+
```
```
      +---------------+
T --> | UnaryOperator | --> T
      +---------------+
```

**Stream API**

**`Stream<T> filter(Predicate<? super T> predicate);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .filter(str -> str.length() > 3);
```
```            
Stream<String>                Stream<String> 
  +-------+                                  
  | one   |                                  
  +-------+                                  
  | two   |                                  
  +-------+                     +-------+    
  | three |     filter()        | three |    
  +-------+  --------------->   +-------+    
  | four  |  str.length() > 3   | four  |    
  +-------+                     +-------+    
  | five  |                     | five  |    
  +-------+                     +-------+          
```

**`<R> Stream<R> map(Function<? super T, ? extends R> mapper);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .map(String::toUpperCase);
```
```
Stream<String>                Stream<String>
  +-------+                     +-------+   
  | one   |                     | ONE   |   
  +-------+                     +-------+   
  | two   |                     | TWO   |   
  +-------+                     +-------+   
  | three |       map()         | THREE |   
  +-------+  --------------->   +-------+   
  | four  |    toUpperCase      | FOUR  |   
  +-------+                     +-------+   
  | five  |                     | FIVE  |   
  +-------+                     +-------+   
```

**`IntStream mapToInt(ToIntFunction<? super T> mapper);`**

```java
Stream.of("1", "2", "3", "4", "5")
            .mapToInt(Integer::valueOf);
```
```
Stream<String>                  IntStream
  +-------+                     +-------+   
  |   1   |                     |   1   |   
  +-------+                     +-------+   
  |   2   |                     |   2   |   
  +-------+                     +-------+   
  |   3   |     mapToInt()      |   3   |   
  +-------+  --------------->   +-------+   
  |   4   |  Integer.valueOf    |   4   | 
  +-------+                     +-------+   
  |   5   |                     |   5   |   
  +-------+                     +-------+   
```

**`LongStream mapToLong(ToLongFunction<? super T> mapper);`**

```java
Stream.of("1", "2", "3", "4", "5")
            .mapToLong(Long::valueOf);
```
```
Stream<String>                 LongStream
  +-------+                     +-------+   
  |   1   |                     |   1   |   
  +-------+                     +-------+   
  |   2   |                     |   2   |   
  +-------+                     +-------+   
  |   3   |     mapToLong()     |   3   |   
  +-------+  --------------->   +-------+   
  |   4   |    Long.valueOf     |   4   | 
  +-------+                     +-------+   
  |   5   |                     |   5   |   
  +-------+                     +-------+   
```

**`DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper);`**

```java
Stream.of("1.1", "2.2", "3.3", "4.4", "5.5")
            .mapToDouble(Double::valueOf);
```
```
Stream<String>                 DoubleStream
  +-------+                     +-------+   
  |  1.1  |                     |  1.1  |   
  +-------+                     +-------+   
  |  2.2  |                     |  2.2  |   
  +-------+                     +-------+   
  |  3.3  |    mapToDouble()    |  3.3  |   
  +-------+  --------------->   +-------+   
  |  4.4  |   Double.valueOf    |  4.4  | 
  +-------+                     +-------+   
  |  5.5  |                     |  5.5  |   
  +-------+                     +-------+   
```

**`<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);`**

```java
Stream.of(Arrays.asList("one", "two"), Arrays.asList("three", "four", "five"))
            .flatMap(list -> list.stream());
```
```
Stream<List<String>                      Stream<String>        
  +-----------+                                                
  | +-------+ |                                                
  | | one   | |                                                
  | +-------+ |                                                
  | | two   | |                            +-------+           
  | +-------+ |                            | one   |           
  |           |         flatMap()          +-------+           
  | +-------+ |      --------------->      | two   |           
  | | three | |                            +-------+           
  | +-------+ |                            | three |           
  | | four  | |                            +-------+           
  | +-------+ |                            | four  |           
  | | five  | |                            +-------+           
  | +-------+ |                            | five  |           
  +-----------+                            +-------+   
```

**`IntStream flatMapToInt(Function<? super T, ? extends IntStream> mapper);`**

*Similar to `flatMap`*

**`LongStream flatMapToLong(Function<? super T, ? extends LongStream> mapper);`**

*Similar to `flatMap`*

**`DoubleStream flatMapToDouble(Function<? super T, ? extends DoubleStream> mapper);`**

*Similar to `flatMap`*

**`Stream<T> distinct();`**

```java
Stream.of("one", "two", "three", "four", "four")
            .distinct();
```
```
Stream<String>                Stream<String>
  +-------+                                
  | one   |                                
  +-------+                     +-------+  
  | two   |                     | one   |  
  +-------+                     +-------+  
  | three |     distinct()      | two   |  
  +-------+  --------------->   +-------+  
  | four  |                     | three |  
  +-------+                     +-------+  
  | four  |                     | four  |  
  +-------+                     +-------+  
```

**`Stream<T> sorted();`**

**`Stream<T> sorted(Comparator<? super T> comparator);`**

**`Stream<T> peek(Consumer<? super T> action);`**

**`Stream<T> limit(long maxSize);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .limit(4);
```

```
Stream<String>                Stream<String>
  +-------+                                
  | one   |                                
  +-------+                     +-------+  
  | two   |                     | one   |  
  +-------+                     +-------+  
  | three |       limit()       | two   |  
  +-------+  --------------->   +-------+  
  | four  |                     | three |  
  +-------+                     +-------+  
  | five  |                     | four  |  
  +-------+                     +-------+  
```

**`Stream<T> skip(long n);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .skip(2);
```
```
Stream<String>                Stream<String>
  +-------+                                
  | one   |                                
  +-------+                      
  | two   |                       
  +-------+                     +-------+  
  | three |       skip()        | three |  
  +-------+  --------------->   +-------+  
  | four  |                     | four  |  
  +-------+                     +-------+  
  | five  |                     | five  |  
  +-------+                     +-------+  
```

**`void forEach(Consumer<? super T> action);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .forEach(str -> {
                System.out.println(str.toUpperCase());
            });
```

**`void forEachOrdered(Consumer<? super T> action);`**

**`Object[] toArray();`**

**`<A> A[] toArray(IntFunction<A[]> generator);`**

**`T reduce(T identity, BinaryOperator<T> accumulator);`**

```java
Stream.of(1, 2, 3, 4)
            .reduce(0, (acc, element) -> acc + element);
```

**`Optional<T> reduce(BinaryOperator<T> accumulator);`**

```java
Stream.of(1, 2, 3, 4)
            .reduce((acc, element) -> acc + element)
            .ifPresent(System.out::println);
```

**`<U> U reduce(U identity,
                BiFunction<U, ? super T, U> accumulator,
                BinaryOperator<U> combiner);`**

*`parallelStream` 的时候 `combiner` 参数才有效。*

```java
Arrays.asList(1, 2, 3, 4, 5, 6).parallelStream()
            .reduce(0,
                (sum, p) -> {
                    System.out.format("accumulator: sum=%s; person=%s\n", sum, p);
                    return sum += p;
                },
                (sum1, sum2) -> {
                    System.out.format("combiner: sum1=%s; sum2=%s\n", sum1, sum2);
                    return sum1 + sum2;
                });
```

**`<R> R collect(Supplier<R> supplier,
                BiConsumer<R, ? super T> accumulator,
                BiConsumer<R, R> combiner);`**
                
**`<R, A> R collect(Collector<? super T, A, R> collector);`**

**`Optional<T> min(Comparator<? super T> comparator);`**

```java
Stream.of(1, 2, 3, 4)
            .min(Comparator.naturalOrder());
```
```
Stream<Integer>           Optional<Integer>  
  +-------+                                  
  |   1   |                                  
  +-------+                                  
  |   2   |                                  
  +-------+                                  
  |   3   |     min()       +-------+        
  +-------+  ----------->   |   1   |        
  |   4   |                 +-------+        
  +-------+                                  
```

**`Optional<T> max(Comparator<? super T> comparator);`**

```java
Stream.of(1, 2, 3, 4)
            .max(Comparator.naturalOrder());
```
```
Stream<Integer>           Optional<Integer>  
  +-------+                                  
  |   1   |                                  
  +-------+                                  
  |   2   |                                  
  +-------+                                  
  |   3   |     max()       +-------+        
  +-------+  ----------->   |   4   |        
  |   4   |                 +-------+        
  +-------+                                  
```

**`long count();`**
```java
Stream.of(1, 2, 3, 4)
            .count();
```
```
Stream<Integer>               long  
  +-------+                                  
  |   1   |                                  
  +-------+                                  
  |   2   |                                  
  +-------+                                  
  |   3   |     count()     +-------+        
  +-------+  ----------->   |   4   |        
  |   4   |                 +-------+        
  +-------+                                  
```

**`boolean anyMatch(Predicate<? super T> predicate);`**

**`boolean allMatch(Predicate<? super T> predicate);`**

**`boolean noneMatch(Predicate<? super T> predicate);`**

**`Optional<T> findFirst();`**

**`Optional<T> findAny();`**
