# ![[tktk Module Name] - tktk Microlesson Name](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to describe what the garbage collector is and how it works.

## Opening (5 min)

We've already seen quite a bit about how Java allocates data, objects, arrays, collections, and more. But we've glossed over a few subtle points:
- Where does Java put all this data?
- We can assume that memory is allocated as needed, but where does that memory come from?
- How does Java manage that memory?
- How does memory get reclaimed when the data is no longer required?

This is where the garbage collector comes in. (Don't worry, it isn't as ominous as it sounds.)

## The Garbage Collector (10 min) 

Before Java entered the picture, earlier languages like C and C++ required the programmer to **allocate** memory as it was needed, then **deallocate** it once it was no longer needed. Java introduced the concept of a **garbage collector**, which relinquished programmers from the duties of basic memory allocation or deallocation. 

The technology behind garbage collection has greatly evolved in the last 20 years or so, and there are large companies that make a career out of optimizing garbage collection. We won't get into the precise details, but the common theme is that the JVM allocates an area of memory called the **heap**, where it stores all objects.
 
When the heap starts to fill up, Java runs a background process (the garbage collector) that looks at every object in the heap and traces its references transitively to determine if they're still directly or indirectly referenced by any live thread. If they're not, then they're eligible for collection.

The garbage collector will mark those for collection and then, in a sweep process, remove that memory and perform a compaction so the memory once again becomes available.

Consider the following program:

```java
for (int i = 0; i < 100; i++) {
    String message = "This is message " + i;
    System.out.println(message);
}
```

> **Knowledge Check**: What happens to the `message` object created during the loop?

<details>
	
<summary>Answer</summary>
	
At the end of each loop iteration, the `message` object created during that iteration is no longer reachable. It wasn't assigned, it has no references, and there's no way to ever get it back.
	
</details>

> **Knowledge Check**: Based on this information, is the `message` object eligible for collection?

<details>
	
<summary>Answer</summary>
	
Yes, it's eligible for garbage collection. 

</details>