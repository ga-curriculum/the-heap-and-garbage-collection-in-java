<h1>
  <span class="headline">The Heap and Garbage Collection in Java</span>
  <span class="subhead">Performance Tuning Basics</span>
</h1>

**Learning objective:** By the end of this lesson, you'll be able to explain how to identify the root cause and resolve memory-related exceptions, namely `OutOfMemoryError` and `StackOverflowError`.

## Introduction to performance tuning

- **Performance Optimization** is the act of making changes to the code to eliminate performance bottlenecks and prevent crashes.
- **Performance Tuning** is the act of customizing the JVM settings for a specific program execution to improve performance and/or prevent crashes.

Performance optimization can be done by following best-practices while coding. Even after that, if we face performance issues, we have to resort to performance tuning. In this lesson, we will learn about performance tuning techniques.

## Tuning the method area (or metaspace)
When JVM's Classloader loads all the classes in the metaspace at the beginning of the program execution, the maximum metaspace memory allocation might be exceeded. This scenario would throw an `OutOfMemoryError: Metaspace` exception. 

### Tuning solution:
We can set the value of maximum possible metadata space to our desired value when launching our progam, as shown in the example below:
```
java -XX:MaxMetaspaceSize=512m MyProgram
```
This command will allocate 512 MB of memory space for Metaspace and run `MyProgram`. 


## Tuning the Stack
The JVM allocates a default stack size for every program during execution, which may be insufficient for deeply recursive or complex methods using a lot of local variables. When the stack memory exceeds this default size, it would throw a `StackOverflowError` exception.

### Tuning solution
We can increase the stack size when launching our program, as shown in the example below:
```
java -Xss1m MyProgram
```
This command will allocate 1 MB of memory space for the stack and run `MyProgram`. 


## Tuning the heap
The heap takes the largest chunk of JVM memory space - about 70 to 80 percent. The performance problems possibly encountered by heap are:
1. Heap also performs dynamic memory resizing, which in itself is a CPU intensive process that could slow down the program execution if didn't start with a sufficient heap size. 
2. Inspite of garbage collection, the heap can still exceed its maximum limit if a large number of objects have live references in the rootset. Once this maximum limit is exceeded, it would throw an `OutOfMemoryError: Java heap space` exception.

### Tuning solution
We can tune the heap size when launching our program, as shown in the example below:

```java
java -Xms10g -Xmx100g MyProgram
```

- `-Xms` is the JVM option for initial heap size. Here, we are setting it to 10 GB so that when `MyProgram` runs, the JVM need not keep dynamically resizing the heap until it reaches 10 GB.
- `-Xmx` is the JVM option for maximum heap size limit. Here, we are setting it to 100 GB so that as long as `MyProgram`'s heap does not exceed 100 GB during its runtime, we will not encounter an `OutOfMemoryError: Java heap space` exception.
- If our `-Xmx` setting is greater than our `-Xms` setting, then Java will start with an initial heap size equal to our `-Xms` setting and allocate more heap as needed until the `-Xmx` value is reached.
