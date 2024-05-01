# ![[tktk Module Name] - tktk Microlesson Name](./assets/outofmemory-error.png)

**Learning objective:** By the end of this lesson, students will be able to examine how to prevent the `OutofMemory` error. 

## Demo: `OutOfMemory` Error (10 min) 

Does this mean Java can never run out of memory? 

What do we think will happen when we run this program?

```java
public class OOMError {
    public static void main(String[] args) {
        List<String[]> list = new ArrayList<>();
        
	for(int i = 0; ; i++) {
            Runtime runtime = Runtime.getRuntime();
            System.out.printf("Iteration: %d total memory: %d free memory: %d%n", i, runtime.totalMemory(), runtime.freeMemory());
            list.add(new String[10_000_000]);
        }
    }
}

```

> **Knowledge Check**: Can someone explain what happened? 

In this program, we continuously create a large array and store it in a `List`. We display the total and available memory on each iteration.

Eventually, because every object is referenced by our `List`, nothing is eligible for garbage collection, and we exhaust the heap. Java runtime throws up its hands with an `OutOfMemory` error.

Here are two reasons a program might ever run out of memory:

1. The program's memory requirements might just exceed the allocated memory. If you have a lot of live data or objects, then you need to allocate enough memory for the program to run. This can be determined by watching the program run in development under a production-like load.
1. You might have a **memory leak**. A memory leak occurs when objects are allocated but not released when done — for example, collecting them without end in a list and never letting them go. Another common cause of memory leaks comes from forgetting to release connection resources, such as database or file system connections. To locate these, review your code, looking at all of your collections to ensure they're not growing without bound and checking all of your connections to ensure you're releasing them when done. Make sure to use Java 7's `try... with` resources syntax as a defense against memory leaks. 

## Resolving an `OutOfMemory` Error (10 min)

So, it's possible to run out of memory. That means we should be asking ourselves, "What do we do when that happens?"

Our memory leak generator is an extreme example of what can happen if our program creates objects with reckless abandon. To prevent this, make sure you're not allowing collections to grow indefinitely.

If your collection belongs to a singleton object or is a static reference, then many objects might be adding to it. If an unlimited number of objects are being added, you need to rethink your program design. 

Some other preventive measures include:
- Reusing duplicates.
- Using least-recently-used structures, which automatically remove items that have not been referenced for a specified amount of time.
- Consider using weak references (see Java's [WeakHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/WeakHashMap.html)), which clean up when memory is running out.

The bottom line is that **fixing** an `OutOfMemory` error is tricky and generally means changing something about the way your program is designed. (Do a quick search on Stack Overflow and you'll see just how many people are trying to figure this out.) It's important to take **preventive measures** when you set up a program to ensure this doesn't happen (and yes, we know — easier said than done).

## Conclusion (5 min) 

Let's check what we covered:
- How does the garbage collector determine what's eligible for collection? 
- How do you set the heap size for a program?
- What happens if you don't set one?
- How does the stack compare to the heap?
- Describe how to prevent an `OutofMemory` error.