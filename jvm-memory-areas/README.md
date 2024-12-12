<h1>
  <span class="headline">The Heap and Garbage Collection in Java</span>
  <span class="subhead">JVM Memory Areas</span>
</h1>

**Learning objective:** By the end of this lesson, ypu'll be able to describe and differentiate heap and stack.

## JVM memory areas
The run-time performance of any Java program is directly influenced by the amount of memory the program consumes. By understanding how the JVM allocates memory, we can write efficient programs by preventing memory leaks, and debugging exceptions like `StackOverflowError`, or `OutOfMemoryError`. When the JVM starts, it allocates a portion of the host machine's memory for running Java applications. This memory is divided into:

- Method area (or metaspace)
- Stack
- Heap memory

## The Method Area (or metaspace)

Once the Java compiler provides the bytecode, the JVM:
- Loads the class data (package name, class name and their bytecode, method names and their bytecode, variable names) into the method area with the help of the classloader.
- Allocates memory for static variables and initializes them with default values (e.g., 0 for integers, null for references).

All these information persist in the method Area throughout the program execution.

### Example
```java
class Animal {
    static int population = 0; // Static variable
    String name;               // Instance variable
}
```
The class definition (`Animal`) and static variable (`population`) are stored in the method area.

### Best practices for improving performance of method area memory
The thumb rule for improving method area performance is reducing memory consumption by minimising or totally avoiding inclusion of unnecessary classes and static variables in our program. We need to limit the number of static variables to only those required for global state or shared resources.


## The Stack
The Stack is a collection of **stack frames**. A stack frame is a unit of memory allocated to each method call. A method call's stack frame contains:
- Method's local variables
- A temporary memory space called **operand stack**
- References (addresses) of objects created during the method execution.
- Reference (address) of the return value (if there is a `return` statement in the method.)

The stack ensures an organized and efficient way to handle method execution and manage temporary data. Its **Last In, First Out (LIFO)** structure allows seamless nested method calls and returns. The JVM:
- **Pushes** a new stack frame onto the stack whenever a method is called.
- **Pops** the stack frame out of the stack after the method finishes execution.
- Returns the flow control to the calling method after the method finishes execution.

### Example
```java
public class Example {
    public static void main(String[] args) {
        int result = add(5, 10);
        System.out.println(result);
    }

    public static int add(int a, int b) {
        return a + b;
    }
}
```
1. When `main` is called, a stack frame for `main` is pushed.
2. Inside `main`, the `add` method is called, so another stack frame for `add` is pushed.
3. When `add` finishes, its frame is popped, and flow control returns to main.


### Best practices for improving performance of stack memory

The stack memory consumption can be limitied by:
- Using iterative solutions instead of recursion when possible. Iterative solutions do not grow the stack as they reuse the same frame.
  - For example:
    ```java
    // Recursive approach
    public int factorialRecursive(int n) {
        if (n == 1) return 1;
        return n * factorialRecursive(n - 1);
    }

    // Iterative approach
    public int factorialIterative(int n) {
        int result = 1;
        for (int i = 1; i <= n; i++) {
            result *= i;
        }
        return result;
    }
    ```
- Using instance variables or heap storage for large data instead of declaring them as local variables.
- Avoiding or minimising nested or chained method calls as they increase stack depth.


## The Heap
Though objects may be created inside method calls, they live beyond the runtime of the method call. The JVM needs a flexible, runtime-managed memory space, independent of the method call's stack frame, to store objects and their data. The Heap provides this dynamic memory allocation, ensuring that applications can handle variable-sized data at runtime. During program execution, whenever an object is created by `new`, the JVM: 
- Allocates a separate memory space for the object in the heap, containing the objects's instance variables and their values.
- Stores a reference to the object in the stack frame of the thread executing the method

### Example
If we instantiate two objects from the `Animal` class as shown below:
```java
public class Example {
    public static void main(String[] args) {
      Animal a1 = new Animal();
      Animal a2 = new Animal();
    }
}

class Animal {
    static int population = 0; // Static variable
    String name;               // Instance variable
}
```
- Each object (`a1`, `a2`) is stored in the Heap.
- The instance variables of these objects, such as `name`, are unique to each object and stored in their respective heap memory area.

### Best practices for improving performance of heap memory
We can improve the performance of heap memory by avoiding creation of unnecessary temporary objects. This also reduces CPU effort spent in object creation and destruction.
