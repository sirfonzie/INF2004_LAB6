# LAB 6: OPTIMISATION & DEBUGGING

**OBJECTIVES**
-	Implement a few optimisation methods
-	Configure and use a debugger for the Raspberry Pi Pico


**EQUIPMENT** 
1.	A laptop that has the Pico C/C++ SDK installed
2.	Raspberry Pico W
3.	Micro-USB Cable

> [NOTE]
> Only students wearing fully covered shoes are allowed in the SR6A lab due to safety.

## **INTRODUCTION** 

This lab focuses on two essential aspects of embedded systems programming with the Raspberry Pi Pico: optimization and debugging. Optimization ensures efficient code execution, while debugging tools and techniques help identify and fix errors in your code. These skills are vital for developing reliable and high-performance embedded applications. Optimization is particularly crucial for embedded systems like the Raspberry Pi Pico due to their limited resources, real-time requirements, and cost considerations. It helps improve performance, efficiency, and cost-effectiveness. Debugging is essential for ensuring that applications on embedded systems work correctly. It helps you find and resolve errors in your code, which is especially important for embedded systems where errors can lead to system crashes or failures. Some examples of optimization include choosing the right data types, using efficient algorithms, optimizing memory usage, and reducing power consumption.

## **OPTIMISATION TECHNIQUES**

In the context of embedded systems, optimization techniques are fundamental practices aimed at enhancing the efficiency and performance of software running on resource-constrained devices like microcontrollers. Code optimization involves refining algorithms, reducing unnecessary operations, and optimizing code structure to ensure faster execution and minimal resource consumption. Memory optimization is crucial for conserving limited memory resources by employing efficient data structures and variable management. Compiler optimization, specifically tailored for embedded systems, fine-tunes code translation to machine code, resulting in improved execution speed and reduced memory usage. These techniques are essential for developing embedded applications that operate efficiently, meet real-time constraints, and make the most of the limited hardware resources available in embedded systems.

Verifying and observing optimization techniques in embedded systems programming, particularly on platforms like the Raspberry Pi Pico, involves a combination of techniques and tools. Here's how you can verify and observe the optimization techniques discussed.


**Code Optimization Techniques**
In this section, you will delve into essential code optimization techniques. Firstly, you will learn about commonly used optimization techniques, such as loop unrolling to reduce loop control overhead, minimizing branching to improve code flow, and optimizing algorithms for enhanced efficiency. Secondly, you will be able to apply these optimization principles in practice. You'll take a sample program and implement the identified optimizations to boost its performance. Finally, you'll employ timing functions to measure and record the program's execution time both before and after optimization. This data will allow you to quantitatively assess the effectiveness of the applied optimizations and gain insights into the performance improvements achieved.

##### 1. Loop Unrolling:
Loop unrolling is a technique that reduces the overhead of loop control instructions by executing multiple iterations of a loop in a single iteration.

**Before Optimization:**

```c
int sum = 0;
for (int i = 0; i < 10; i++) {
    sum += i;
}
```

**After Loop Unrolling:**

```c
int sum = 0;
for (int i = 0; i < 10; i += 2) {
    sum += i;
    sum += i + 1;
}
```

In this example, loop unrolling reduces the number of loop control instructions by processing two iterations at a time, which can lead to improved performance.


##### 2. Minimize Branching:
Reduce branching instructions in your code as they can introduce overhead due to potential branch mispredictions.

**Before Optimization:**

```c
if (condition) {
    // Code block A
} else {
    // Code block B
}
```

**After Optimization:**

```c
// Code block A
if (!condition) {
    // Code block B
}
```

In this example, we have reduced the branching overhead by moving the common code block outside of the `if` statement.


##### 3. Algorithm Optimization:
Optimize algorithms to reduce time complexity. For instance, replacing a linear search with a binary search for sorted data can significantly improve performance.

**Before Optimization:**

```c
int linear_search(int arr[], int n, int target) {
    for (int i = 0; i < n; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}
```

**After Optimization:**

```c
int binary_search(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            return mid;
        }
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

In this example, replacing a linear search with a binary search improves the algorithm's time complexity from O(n) to O(log n).



### 1. Execution Time Measurement:

#### Before Optimization:
1. Measure the execution time of your program before applying any optimizations.
2. Use the `pico_time.h` library or other timing functions to record the time taken by specific code sections or the entire program.

#### After Optimization:
1. Re-run the program with the optimizations applied.
2. Measure and record the execution time again using the same timing functions.

#### Observation:
- Compare the execution times before and after optimization. If the execution time has significantly reduced, it indicates the effectiveness of your optimizations.

**Memory Optimization**
1. Explore memory optimization techniques for minimizing variable sizes and efficient data structures.
2. Modify a program to reduce memory usage while preserving functionality.
3. Monitor memory usage using `pico_malloc_stats` or similar functions to assess memory optimization success.

### 3.2 Memory Optimization

#### Memory Optimization Techniques:

##### 1. Minimize Variable Sizes:

Reduce the size of data types wherever possible to minimize memory usage.

**Before Optimization:**

```c
struct SensorData {
    float temperature;
    float humidity;
};
```

**After Optimization:**

```c
struct SensorData {
    uint16_t temperature; // Assuming temperature range can be represented with uint16_t
    uint16_t humidity;    // Assuming humidity range can be represented with uint16_t
};
```

In this example, we changed `float` to `uint16_t` if the data range allows it, saving memory.

##### 2. Efficient Data Structures:

Choose data structures that use memory efficiently. For example, use arrays instead of linked lists when memory usage is a concern.

**Before Optimization:**

```c
struct Node {
    int data;
    struct Node* next;
};
```

**After Optimization:**

```c
int array[SIZE];
```

In this example, using an array instead of a linked list for a large dataset saves memory and reduces overhead.

### 2. Memory Usage Monitoring:

#### Before Optimization:
1. Use functions like `pico_malloc_stats` to monitor memory usage before optimization.
2. Record the memory statistics, including heap and stack usage.

#### After Optimization:
1. Rerun the program with the optimizations in place.
2. Monitor memory usage using the same memory profiling functions.

#### Observation:
- Compare memory statistics before and after optimization. A reduction in memory usage, especially in the heap, indicates successful memory optimization.


**Compiler Optimization Flags**
1. Understand compiler optimization flags like `-O1`, `-O2`, and `-O3` and their impact on code optimization.
2. Compile a program with different optimization levels and compare their performance.
3. Discuss trade-offs between code size and execution speed with different optimization levels.

### 3.3 Compiler Optimization Flags

#### Compiler Optimization Flags:
Optimizing code during compilation can be as simple as specifying compiler flags. The level of optimization depends on the flag used:

**No Optimization:**

```bash
gcc -O0 my_program.c -o my_program
```

**Moderate Optimization:**

```bash
gcc -O1 my_program.c -o my_program
```

**High Optimization:**

```bash
gcc -O2 my_program.c -o my_program
```

**Maximum Optimization (Aggressive):**

```bash
gcc -O3 my_program.c -o my_program
```

Using higher optimization levels can significantly improve code performance but may also increase code size. Choose the appropriate level based on your project requirements.

These code optimization techniques are fundamental for improving the efficiency and performance of embedded systems programs running on Raspberry Pi Pico or similar microcontrollers.


### 3. Compiler Optimization Flags:

#### Before Optimization:
1. Compile your code without any optimization flags using the `-O0` flag.
2. Generate the executable binary.

#### After Optimization:
1. Recompile the code with different optimization flags (`-O1`, `-O2`, `-O3`) one at a time.
2. Generate separate executable binaries for each optimization level.

#### Observation:
- Execute each binary and record the execution time and memory usage for each optimization level.
- Compare the results to determine which optimization level yields the best performance while considering memory constraints.

### 4. Visual Inspection:

#### Before Optimization:
1. Review your code for potential optimizations, such as loop unrolling, minimizing branches, and algorithm improvements.
2. Identify code segments that can be optimized.

#### After Optimization:
1. Implement the identified optimizations and document the changes made in the code.
2. Use comments or version control tools to track changes.

#### Observation:
- Visual inspection helps you identify and understand the specific optimizations applied to your code.

### 5. Profiling and Benchmarking:

#### Before and After Optimization:
1. Utilize profiling tools and benchmarking frameworks specific to your platform and programming language.
2. Profile your code to identify performance bottlenecks and resource usage before and after optimization.

#### Observation:
- Profiling tools provide detailed information about which parts of your code consume the most resources or have the highest execution time. Use this data to pinpoint areas that require optimization.

### 6. Test Cases:

#### Before and After Optimization:
1. Develop and execute test cases that represent real-world usage scenarios for your application.
2. Ensure that the test cases cover various input conditions and use cases.

#### Observation:
- Observe how your program performs with the optimization changes when subjected to the test cases. Look for improvements in execution time, memory usage, and overall program stability.

### 7. Debugging:

#### After Optimization:
1. Debug your code thoroughly after applying optimizations to identify and rectify any new issues or unintended consequences introduced by the optimizations.

#### Observation:
- Debugging helps ensure that optimizations do not introduce new bugs or issues into your code.

By following these verification and observation techniques, you can effectively assess the impact of optimization techniques on your embedded systems code running on the Raspberry Pi Pico or similar platforms.
---



## Debugging Tools and Techniques

### LED Debugging
1. Use an LED connected to the Raspberry Pi Pico to provide visual feedback during program execution.
2. Implement LED blinking patterns to indicate specific program states or errors.
3. Debug your code by observing the LED's behavior.

### Serial Debugging with UART

1. Set up UART communication between the Raspberry Pi Pico and your computer.
2. Send debugging information, including variable values, status updates and execution progress, via UART (e.g via `printf`).
3. Use a serial terminal or debugging tool on your computer to receive and analyze the UART output for debugging.

---

Remember that optimization and debugging are ongoing processes, and the skills you've learned in this lab will be essential for developing reliable and efficient embedded systems in the future.

---

## Additional Resources

[Raspberry Pi Pico SDK Documentation](https://raspberrypi.github.io/pico-sdk-doxygen/index.html)

