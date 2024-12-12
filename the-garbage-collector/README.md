<h1>
  <span class="headline">The Heap and Garbage Collection in Java</span>
  <span class="subhead">The Garbage Collector</span>
</h1>

**Learning objective:** By the end of this lesson, you'll be able to describe what the garbage collector is and how it works.

## Memory leakage
Memory leakage occurs when a program fails to release memory that is no longer needed, leading to reduced available memory over time. In an OOP laguage like Java, this typically happens when objects are not properly de-referenced or released, even though they are no longer in use.

### Implications
- **Performance degradation**: Over a long execution time, the application consumes increasing amounts of memory, leading to reduced performance and sluggishness.
- **Application crash**: If the memory leak persists, the program might exhaust available memory, causing crashes.
- **Resource Starvation**: Memory leaks can cause other processes on the same system to experience resource shortages, affecting overall system stability.

## The Garbage Collector
In early OOP languages like C++, it was the responsibility of the programmer to explicitly allocate and deallocate memory spaces for objects using program statements. In significantly complex programs, there was always the risk of memory leakage due to human errors - forgetting to code statements to release object memory spaces once they are no more required.

The **Garbage Collector** (GC) is a memory management tool in JVM's kitty designed to automatically reclaim heap memory occupied by objects no longer accessible in a program. The garbage collector:
- **Prevents Memory Leaks**: By identifying and removing unused objects, the GC ensures that memory is not wasted.
- **Simplifes Development**: Java developers do not need to manually allocate and deallocate memory, as the GC automates this process.
- **Enhances Application Stability**: By managing memory efficiently, the GC helps reduce the likelihood of errors such as OutOfMemoryError.

## How garbage collector works

### The parts involved

- **Young generation region**: This is a region in the heap where every newly instantiated object resides.
- **Old generation region**: Objects that have survived a preset number of garbage collection cycles in the young generation region are move to this region.
- **Trigger settings**: The young generation region, the old generation region and the entire heap, have their individual GC trigger thresholds set by the JVM. This trigger setting is a value in terms of memory size. Once this size is reached, the garbage collection process that specific region is triggered.
- **Garbage collector rootset**: For garbage collector, roots are different parts of the JVM memory space which store object references (addresses of the object memory space in the heap).
  - **Stack roots**: Unpopped stack frames in the stack. They represent methods that are still alive in the program execution. Their local variables and method arguments may contain object references as their values. For example:
    ```java
      public void example() {
        String str = "Hello";  // Reference to a String object in the heap
      }
    ```
  - **Global roots**: Found in the Method Area, which stores class-level information. Their static members may contain object references as their values. For example:
    ```java
      public class Example {
        private static MyObject obj = new MyObject();  // Static reference
    }
    ```
- ***Object Map**: The Object Map is present in the heap acts as a record keeper of all objects stored in the heap. Each entry in the Object Map corresponds to an object allocated in the heap, providing information like:
  - Object's address in memory.
  - Type of the object (its class).
  - Size of the object in the heap.


### The process

#### Step 1: Trigger phase
The garbage collection process is kickstarted when the JVM senses one of the heap memories (young generation, old generation or the entire heap) reaches its trigger thresholds. The garbage collector is invoked and starts acting on the area whose trigger threshold was reached.

#### Step 2: Mark phase
The garbage collector compares the object references present in the rootset and the object map in the heap. Based on matches found it **marks** the objects in the object map as:
- **Live objects**: Objects that are still in use (reachable from root references).
- **Garbage objects**: Objects that are no longer referenced in any root and, hence, not used anymore.

#### Step 3: Pause phase
The JVM temporarily pauses the program events execution to ensure no changes occur in object references. This prepares the garbage collector for sweep phase.

#### Step 4: Sweep phase
- Unreachable objects are deleted, and their memory is marked as free. The space is added back to the heap for reuse.
- To reduce data fragmentation, reachable objects are copied to a new location (often compacted together).
The old memory area is cleared entirely.

#### Step 5: Move phase (optional)
This step occurs whenever the garbage collector marks and sweeps the young generation region (only when the gargabe collection is triggered by young generation threshold or entire heap threshold) Objects that survive a preset number of GC cycles are moved from the Young Generation to the Old Generation, assuming they are long-lived.

#### Step 6: Restart phase
The memory previously occupied by unreachable objects is now available for reuse. The program execution is continued from the place where it was paused, with more free memory.