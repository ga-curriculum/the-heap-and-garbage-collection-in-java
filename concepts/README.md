<h1>
  <span class="headline">The Heap and Garbage Collection in Java</span>
  <span class="subhead">Concepts</span>
</h1>

**Learning objective:** By the end of this lesson, you'll be able to describe the Java Virtual Machine's (JVM) purpose and functions.

## The Java Virtual Machine (JVM)
One of Java's biggest strengths is its **“Write Once, Run Anywhere”** philosophy. The JVM enables Java programs to run on different platforms without modification. We can develop a Java application on Windows and run it on Linux or macOS without rewriting or recompiling the code, as long as a compatible JVM exists on the target platform. A clear understanding of JVM execution helps programmers make informed decisions about resource usage and program efficiency.


### How a Java program is executed by JVM
- **STEP 1**: The Source code (`.java file`) is created by the programmer using Java Syntax.
- **STEP 2**: The Java compiler converts the `.java` file into a `.class` file. The `.class` file contains the bytecode for the program. Bytecode is the language in which JVM can execute a program.
- **STEP 3**: The **JVM's Classloader** finds and loads all the `.class files` into **JVM's memory areas**. 
- **STEP 4**: The **JVM's Execution Engine**: Converts the bytecode into machine instructions, which are passed to host machine CPU for execution.


## Portability of Java applications
Let's take a deepdive into how JVM makes Java programs portable. To do this, lets learn one of the fundamental skills of Java programmers - Running a java program in Bash (using shell commands). You can try the following steps on any machine with any operating system (Windows, macOS or Linux) that already has Java installed.

### Prerequisite
Verify whether Java is installed in your system by executing the following commands in your machine's terminal emulator (Terminal in case your are a Mac user, and Command Prompt in case you are a Windows user).
```
javac -version
java -version
```
If Java is installed in your system, both the above commands will give you the respective version numbers. `javac` command invokes the Java compiler and `java` command invokes the Java Virtual Machine.

### Step-by-step guide
- **Step 1**: Using any plaintext editor, create a file named `HelloWorld.java` with the following content:
    ```java
    public class HelloWorld {
      public static void main(String[] args) {
        System.out.println("Hello, World!");
      }
    }
    ```
- **Step 2**: Save this file in a directory. For example, `~/JavaProjects/`.
- **Step 3**: Open the terminal emulator in your machine. Navigate to the directory containing our `HelloWorld.java` file using the `cd` command.
  ```
  cd ~/JavaProjects/
  ```
- **Step 4**: Java source files (`.java`) need to be compiled into bytecode (`.class`) before they can be executed. This can be done by invoking the Java compiler and providing our source file as its argument, using the following command:
  ```
  javac HelloWorld.java
  ```
  This command creates a file named `HelloWorld.class` in the same directory, which contains the bytecode of our program. This is a portable file containing the bytecode that will run on any machine (Windows, mac or Linux) that has a JVM.
- **Step 5**: Lets test in it on our own machine by running the bytecode by invoking the JVM using the following command:
  ```
  java HelloWorld
  ```
  Note that we need not provide the `.class` file extention here. If we get the below output, it means our program ran successfully:
  ```
  Hello, World!
  ```

### Key Takeaway
One of the primary responsibilities of JVM is to make syntactically correct java programs executable on any operating system without worrying about any additional manipulation, configuration or coding for ensuring compatibility. The JVM acts as a virtual machine between the Java application and the underlying operating system, translating platform-independent bytecode into platform-specific machine code.


