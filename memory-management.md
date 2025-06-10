---
# Memory Management
---

Memory management is how a computer program requests, uses, and releases memory (RAM) during its execution.

Memory Management is controlled by:

## 1. Operating System
- Manages overall system memory
- Allocates memory to different programs
- **Example:** Windows allocates 2GB to your web browser, 1GB to your music player

## 2. Programming Language and Runtime Environment
- Manages memory within your specific program
- Allocates memory for variables, objects, and data structures
- **Example:** Java Virtual Machine manages memory for your Java program

## Key Activities of Memory Management

- Memory allocation
- Memory deallocation
- Garbage collection
- Use of pointers for memory access
- Ensuring memory safety

Memory is requested by a program from the OS (when it is needed), which can be allocated at compile time (statically) or at runtime (dynamically).

## Memory Allocation Types

| | **EXPLICIT ALLOCATION** | **IMPLICIT ALLOCATION** |
|---|---|---|
| **EXPLICIT DEALLOCATION** | C language<br/>malloc()<br/>free() | (Not common) |
| **IMPLICIT DEALLOCATION** | (Not common) | Java language<br/>Python language<br/>new operator<br/>gc thread |

Memory, when no longer required by the program, needs to be deallocated and returned to the OS. If not, they can result in memory leaks.

Memory leaks slow down program execution and ultimately lead to program crashes.

## Garbage Collector
- Automatically detects and frees memory that is no longer in use
- Traces through a program's data structures to find all the objects. Any memory that is not reachable is considered as garbage

### Garbage Collection Techniques

1. **Reference counting:** Keeps track of the number of references to each object
2. **Tracing:** Uses a set of memory roots and recursively follows all the pointers to other objects in memory

## Memory Layout

### 1. Text Segment
- **Location:** Bottom of the memory
- **Purpose:** Store machine code
- **Characteristics:** Read-only, Shared, Fixed size

### 2. Initialized Data Segment
- **Location:** Above Text segment
- **Purpose:** Store global and static variables that have initial values
- **Characteristics:** Read-write, loaded from executable

### 3. Uninitialized Data Segment
- **Purpose:** Stores global and static variables that are not initialized or initialized to zero
- **Characteristics:** 
  - Initialized to zero: OS automatically sets all values to 0
  - Not stored in executable file: Saves file space

### 4. Heap Segment
- **Location:** Above BSS, grows upward toward higher addresses
- **Purpose:** Dynamic memory allocation during runtime
- **Characteristics:** Grows upward, Managed by programmer, Flexible size

### 5. Stack Segment
- **Location:** Top of memory (highest addresses), grows downward
- **Purpose:** Function calls, local variables, function parameters
- **Characteristics:** Grows downward, Automatic management, LIFO: Last In, First Out

```
High address
┌─────────────────────────────────────────────┐
│                  Stack                      │  ← grows downward
├─────────────────────────────────────────────┤
│                  Heap                       │  ← grows upward
├─────────────────────────────────────────────┤
│           Uninitialized data (BSS)          │  ← Initialized to zero by exec
├─────────────────────────────────────────────┤
│             Initialized data                │  ← Read from program file by exec
├─────────────────────────────────────────────┤
│                  Text                       │  ← Read-only code segment
└─────────────────────────────────────────────┘
Low address
```

## Memory Issues

### Stack Issues
- **Stack overflow:** Too many function calls or large local arrays
- **Dangling pointers:** Returning pointer to local variable

### Heap Issues
- **Memory leaks:** Forgetting to free allocated memory
- **Double free:** Freeing same memory twice
- **Fragmentation:** Memory becomes scattered

### Data Segment Issues
- **Buffer overflow:** Writing beyond array boundaries
- **Uninitialized variables:** Using variables before setting values

## C Memory Allocation

### TEST 01: Basic Memory Segments

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {
    int i = 1;        // STACK (not initialized data!)
    int j;            // STACK (not uninitialized data!)
    int *ptr;         // STACK (pointer variable)
    ptr = malloc(sizeof(int)); // HEAP allocation
    
    printf("%i %i %i\n", i, j, *ptr);     // 1, garbage, garbage
    i = 2;
    *ptr = 3;
    printf("%i %i %i\n", i, j, *ptr);     // 2, garbage, 3
    j = 4;
    free(ptr);
    printf("%i %i %i\n", i, j, *ptr);     // 2, 4, UNDEFINED!
}
```

#### Issues:
1. **Uninitialized variables:** j and *ptr contain garbage values
2. **Use after free:** *ptr after free(ptr) is undefined behavior (crash risk)

#### Expected Output:
```
1 [garbage] [garbage]
2 [garbage] 3
2 4 [undefined - program may crash]
```

### TEST 02: Pointer Pass-by-Value (BUGGY!)

```c
void func2(int *ptr1) {    // Copy of pointer value
    int *ptr2;             // Uninitialized local pointer
    printf("%i %i\n", *ptr1, *ptr2);        // CRASH! Both uninitialized
    ptr1 = malloc(sizeof(int));              // Only changes local copy
    ptr2 = malloc(sizeof(int));              // Memory leak!
    printf("%i %i\n", *ptr1, *ptr2);        // Still garbage values
}

void test2() {
    int *ptr1;             // Uninitialized pointer on STACK
    printf("%i\n", *ptr1); // CRASH! Dereferencing garbage
    func2(ptr1);           // Passing garbage pointer
    printf("%i\n", *ptr1); // Still garbage - unchanged
    free(ptr1);            // CRASH! Freeing invalid pointer
    printf("%i\n", *ptr1); // CRASH! Use after invalid free
}
```

#### Major Problems:
1. **Uninitialized pointers:** ptr1 contains garbage address
2. **Dereferencing garbage:** *ptr1 and *ptr2 access random memory
3. **Pass-by-value:** Changes to ptr1 in func2 don't affect original
4. **Memory leaks:** malloc in func2 is never freed
5. **Invalid free:** Trying to free uninitialized pointer

### TEST 03: Pointer Pass-by-Reference

```c
void func3(int **ptr1) {   // Pointer to pointer (reference)
    int *ptr2;             // Uninitialized local pointer
    printf("%i %i\n", **ptr1, *ptr2);       // CRASH! Both uninitialized
    (*ptr1) = malloc(sizeof(int));           // Actually changes original ptr1
    ptr2 = malloc(sizeof(int));              // Memory leak!
    printf("%i %i\n", **ptr1, *ptr2);       // Still garbage values
}

void test3() {
    int *ptr1;             // Uninitialized pointer
    printf("%i\n", *ptr1); // CRASH! Dereferencing garbage
    func3(&ptr1);          // Pass address of ptr1
    printf("%i\n", *ptr1); // Now points to allocated memory (garbage value)
    free(ptr1);            // This free is valid
    printf("%i\n", *ptr1); // CRASH! Use after free
}
```

#### Still Has Problems:
1. **Initial dereferencing:** Crashes before func3 is called
2. **Uninitialized memory:** malloc doesn't initialize values
3. **Memory leak:** ptr2 allocation is never freed
4. **Use after free:** Dereferencing ptr1 after free

## Java Memory Allocation Example

```java
class MemoryTest {
    private int i;                    // Instance variable
    
    public MemoryTest(int i) {        // Constructor
        this.i = i;
    }
    
    public void print() {
        System.out.println(i);
    }
    
    public static void main(String[] args) {
        MemoryTest mt;                // Reference variable
        mt = new MemoryTest(1040);    // Object creation
        mt.print();                   // Method call
    }
}
```

### Memory Management Comparison

| **Aspect** | **C** | **Java** |
|---|---|---|
| **Allocation** | Manual (malloc) | Automatic (new) |
| **Deallocation** | Manual (free) | Automatic (Garbage Collection) |
| **Memory Leaks** | Common | Rare (GC handles it) |
| **Pointers** | Direct memory addresses | References (safer) |

## Achieving Memory Efficiency

- Minimize the amount of memory used by a program
- Minimize the amount of work the garbage collector has to do

### Typical Object Metadata

- **Class pointer** - pointer to class information, for example, Integer or String
- **Flags** - object shape, hash code, etc
- **Lock** - lock variable or a pointer to a monitor object used to control concurrent access
- **Size** - the length of the array for array-type objects

**Note:** int variable to Integer object size ratio is 1:4

## Java String Concatenation and Temporary Objects

```java
public static void main(String[] args) {
    // explicit string object creation
    String s1 = new String("Program ");
    // implicit string object creation
    String s2 = "Construction";
    // string concatenation using + operator
    String s3 = s1 + s2; // (Q)
}
```

Java compiler transforms (Q) as:
```java
String s3 = new StringBuilder(String.valueOf(s1)).append(s2).toString();
```

### Heap Memory:

```
┌─────────────────────────────────────┐
│ s1: String("Program ")              │ ← Permanent
├─────────────────────────────────────┤
│ s2: String("Construction")          │ ← Permanent  
├─────────────────────────────────────┤
│ StringBuilder (temporary)           │ ← Temporary (eligible for GC)
├─────────────────────────────────────┤
│ s3: String("Program Construction")  │ ← Permanent
└─────────────────────────────────────┘
```

## Summary

- An object is much larger than the item(s) of data stored in it
- An object encapsulates a data item allowing Java run-time to provide:
  - Memory management
  - Data synchronization
- It also allows (debugging) tools to analyze and visualize applications
- A 64-bit system would have a greater negative effect on memory usage
- Scope of variables referencing objects must be carefully handled to ensure performance efficiency of the GC process
- Memory allocation and deallocation can be reduced using caching with only referencing required
- Overall memory requirement can be reduced with caching by sharing rather than copying
