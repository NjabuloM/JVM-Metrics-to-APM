# JVM Metrics | 101 Guide to JVM Metrics Monitoring
Paying attention to JVM metrics isn't solely about numbers; it's essential for ensuring efficient resource usage, optimizing performance, and troubleshooting issues. In this guide, we'll explore the crucial JVM metrics worth monitoring and methods for collecting them.

## JVM refresher, i.e. what is it?
The JVM (Java Virtual Machine) is a sophisticated software layer that enables the platform-independent execution of Java programs by providing a runtime environment, memory management, performance optimization, and security mechanisms. It plays a crucial role in making Java a versatile programming language for a variety of applications.

## Key JVM metrics to monitor
Pivotal JVM metrics offer insights into various aspects of JVM behavior and assist in diagnosing issues. Below is a list of some of these metrics and the information they provide.

    - `Heap Memory Usage`: indicates the amount of memory currently allocated for Java objects and data structures in the heap
    - `Non-Heap Memory Usage`: refers to the amount of memory consumed by areas outside the heap, including class metadata, thread stacks, and native memory allocations.
    - `Garbage Collection Metrics`: show the effectiveness of how the memory is managed
    - `Metaspace Metrics`: provide insights into the memory utilization, capacity, allocation rate, and other VM internal data.
    - `Thread Metrics`: shows statistics about the threads running in the JVM.
    - `Class Loading Metrics`: typically show statistics related to the loading of classes during the execution of a Java program.
    - `CPU Utilization Metrics`: show the amount of CPU resources consumed by the JVM process itself.
    - `I/O Metrics`: refer to statistics related to input and output operations performed by Java applications.

## Closer look at each metric
The JDK (Java Development Kit) comes pre-packaged with a handful of tools for interacting with these metrics, including both GUI-based tools (e.g., JConsole) and Terminal/Console-based tools (e.g., jstat, jstack, jcmd, and jinfo). Below is a demonstration of how these tools can be used to query the listed metrics.

> To launch JConsole, navigate to the 'bin' directory of your JDK installation using the 'cd' command. Once in the 'bin' directory, use the 'jconsole' command to launch JConsole.

### Heap Memory Usage
Using JConsole, select the 'pid' (process ID) of the running JVM instance (local or remote). Then, selecting the 'Memory' tab displays the relevant memory information. The 'Chart' drop-down menu provides access to various other options.

![Heap Memory usage as shown on JConsole](/JConsole_Memory_View.png)

Querying the same information from the Terminal or Command Prompt, jstat (which is also available in the 'bin' directory of the JDK installation) can be used as follows:

```
jstat <option> <pid> <interval> <count>
```

- `<option>`: Options flag that can be any of these:
  - class
  - compiler
  - gc
  - gccapacity
  - gccause
  - gcmetacapacity
  - gcnew
  - gcnewcapacity
  - gcold
  - gcoldcapacity
  - gcutil
  - printcompilation
- `<pid>`: The process ID of the JVM to be monitored
- `<interval>`: Sampling interval ["ms"|"s"]
- `<count>`: Number of samples to take before terminating.

Here is an example query using the `gc` option, along with the output:

```
jstat -gc 5117 1000 10
```

|S0C | S1C    | S0U | S1U    | EC      | EU     | OC      | OU      | MC      | MU      | CCSC   | CCSU   | YGC | YGCT  | FGC| FGCT  | CGC | CGCT  | GCT   |
|----|--------|-----|--------|---------|--------|---------|---------|---------|---------|--------|--------|-----|-------|----|-------|-----|-------|-------|
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |
|0,0 | 2048,0 | 0,0 | 1488,3 | 17408,0 | 5120,0 | 25600,0 | 14664,0 | 34496,0 | 33846,7 | 4480,0 | 4175,2 | 8   | 0,043 | 0  | 0,000 | 2   | 0,002 | 0,046 |


> Please find below the legends and/or descriptions corresponding to the headings of the table above.


| Heading acronym    | Description |
| :---               |    :----:   |
| S0C, S1C, S0U, S1U | current capacity and usage of Survivor Spaces 0 and 1, which are regions within the JVM used for temporary storage of objects surviving minor garbage collection cycles. |
| EC, EU             | the current capacity (EC) and usage (EU) of the Eden Space, which is the region within the JVM's heap where new objects are initially allocated. |
| OC, OU             | indicates the current capacity (OC) and usage (OU) of the Old Generation, which is the region within the JVM's heap where long-lived objects are stored. |
| MC, MU             |  shows the current capacity (MC) and usage (MU) of the Metaspace, which is the memory area within the JVM responsible for storing class metadata. |
| CCSC, CCSU         | indicates the current capacity (CCSC) and usage (CCSU) of the Survivor Space, which is a region within the JVM's heap used for temporary storage of objects that have survived minor garbage collection cycles. |
| YGC, YGCT          | the number of Young Generation garbage collection events (YGC) and the time taken for Young Generation garbage collection (YGCT) |
| FGC, FGCT          | shows the number of Full Garbage Collection events (FGC) and the time taken for Full Garbage Collection (FGCT) |
| CGC, CGCT          | indicates the number of Concurrent Garbage Collection events (CGC) and the time taken for Concurrent Garbage Collection (CGCT) |
| GCT                | Total Garbage Collection Time, which represents the cumulative time taken for all garbage collection events, including Young Generation, Full Garbage Collection, and Concurrent Garbage Collection |

### Non-Heap Memory Usage
// TODO:

### Garbage Collection Metrics
// TODO:

### Metaspace
// TODO:

### Size of metaspace
// TODO:

### Thread Metrics
// TODO:

### Class Loading Metrics
// TODO:

### CPU Utilization
// TODO:

### I/O Metrics
// TODO:

## Collecting JVM metrics
// TODO: This is the part where you show how to instrument and send to SigNoz

## Conclusion
// TODO: Closing off comments