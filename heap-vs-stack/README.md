# ![[tktk Module Name] - tktk Microlesson Name](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to differentiate between the heap and the stack.

## Heap vs. Stack (10 min) 

You can specify the heap size when you launch your program using launch flags:

```sbtshell
-Xms allocates the initial (and minimum) heap size.
-Xmx allocates the maximum heap size. 
```

For example, the following will allocate an initial memory of 10 GB and increase that to 100 GB:

```java
java -Xms10g -Xmx100g MyCode
```

If your `-Xmx` setting is greater than your `-Xms` setting, then Java will start with an initial heap size equal to your `-Xms` setting and allocate more heap as needed until the `-Xmx` value is reached. 

#### Default Heap Size

If you don't specify a heap size, Java will allocate a default heap size for you based on your machine's physical memory. However, it's a good practice to determine your maximum expected heap size by observing the program during execution using tools like Linux's `ps` or `top`.

#### The Stack

Note that, when your program executes, even if there isn't a single object allocated, it still uses some memory to keep track of the call stack of every thread running.

For example, if Method A calls Method B calls Method C, Java must remember that, when Method C exits, it must return to the spot in Method B that called it and, when that returns, return to Method A. This memory is called the **stack** and is distinct from the heap. 

The stack also holds any method-local data that will be swept off once the method returns. The stack works on a 
"last in, first out" (aka LIFO) basis: It's allocated and data is assigned, then methods call other methods, allocating more stack space. Once a method returns, it is popped from the stack so the next frame in the stack (representing the calling method) becomes the top of the stack, and so on.
