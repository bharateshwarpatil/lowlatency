## Activities

The field of performance includes the following activities, listed in an ideal order of
execution:

1. Setting performance objectives and performance modeling
2. Performance characterization of prototype software or hardware
3. Performance analysis of development code, pre-integration
4. Performing non-regression testing of software builds, pre- or post-release
5. Benchmarking/benchmarketing for software releases
6. Proof-of-concept testing in the target environment
7. Configuration optimization for production deployment
8. Monitoring of running production software
9. Performance analysis of issues

###  CPU Performance 

CPU Performance Counters
 They typically include counters for the following:
 
 CPU cycles: including stall cycles and types of stall cycles

 CPU instructions: retired (executed)

 Level 1, 2, 3 cache accesses: hits, misses

 Floating-point unit: operations

 Memory I/O: reads, writes, stall cycles

 Resource I/O: reads, writes, stall cycle

Since performance counters vary among manufacturers, a standard has been developed to provide a consistent interface across them. This is the Processor Application Programmers Interface (PAPI). Instead of the Intel names seen in
PAPI assigns generic names to the counter types, for example, PAPI_tot_cyc for total cycle counts, instead of CPU_CLK_UNHALTED

Example:' 
Event Select  UMASK         Unit           Name            Description
0x43           0x00       data cache   DATA_MEM_REFS        All loads from any memory type. All stores to any memory type. Each part of a split is counted separately. . . .Does not include I/O accesses or other nonmemory accesses.

## Scheduling 
A brief summary of how scheduling works for recent Linux and Solaris-based kernels follows. Function names are included, so that you can find them in the
source code for further reference (although they may have changed). Also refer to internals texts, listed in the Bibliography.

## Linux
On Linux, time sharing is driven by the system timer interrupt by calling scheduler_tick(), which calls scheduler class functions to manage priorities
and the expiry of units of CPU time called time slices. Preemption is triggered when threads become runnable and the scheduler class check_preempt_curr()
function is called. Switching of threads is managed by __schedule(), which selects the highest-priority thread via pick_next_task() for running. Load balancing is performed by the load_balance() function.

### uptime: 
 Check load averages to see if CPU load is increasing or decreasing over time. A load average over the number of CPUs in the system usually indicates saturation

### vmstat: 
 Run vmstat per second, and check the idle columns to see how much headroom there is. Less than 10% can be a problem
 
###  mpstat: 
Check for individual hot (busy) CPUs, identifying a possible thread scalability problem

### top/prstat: 
 See which processes and users are the top CPU consumers.
 
### perf/dtrace/stap/oprofile 
Profile CPU usage stack traces for either user- or kernel-time, to identify why the CPUs are in use

## USE Method

The USE method is for identifying bottlenecks and errors across all components, early in a performance investigation, before deeper and more time-consuming
strategies are followed.
For each CPU, check for

####  Utilization: the time the CPU was busy (not in the idle thread)
####  Saturation: the degree to which runnable threads are queued waiting their turn on-CPU
####  Errors: CPU errors, including correctable errors

## Work load characterisation 
Load averages (utilization + saturation)
Using the uptime command check load averages to see if CPU load is increasing or decreasing over time. A load average over the number of CPUs in the system usually indicates saturation


## User-time to system-time ratio
High user-time rates are due to applications spending time performing their own compute. High system-time shows time spent in the kernel instead, which may be further understood by the syscall and interrupt rate. I/O-bound workloads have higher system-time, sys-calls, and also voluntary context switches as threads block waiting for I/O

What is the CPU utilization system-wide? Per CPU?
 How parallel is the CPU load? Is it single-threaded? How many threads?
 Which applications or users are using the CPUs? How much?
 Which kernel threads are using the CPUs? How much?
 What is the CPU usage of interrupts?
 What is the CPU interconnect utilization?
 Why are the CPUs being used (user- and kernel-level call paths)?
 What types of stall cycles are encountered?
 
## Profiling

Profiling builds a picture of the target for study. CPU usage can be profiled by
sampling the state of the CPUs at timed intervals, following these steps:
1. Select the type of profile data to capture, and the rate.
2. Begin sampling at a timed interval.
3. Wait while the activity of interest occurs.
4. End sampling and collect sample data.
5. Process the data.

 
 ## Static Performance Tuning
Static performance tuning focuses on issues of the configured environment. For CPU performance, examine the following aspects of the static configuration:

 How many CPUs are available for use? Are they cores? Hardware threads?
 Is the CPU architecture single- or multiprocessor?
 What is the size of the CPU caches? Are they shared?
  What is the CPU clock speed? Is it dynamic (e.g., Intel Turbo Boost and SpeedStep)? Are those dynamic features enabled in the BIOS?
 What other CPU-related features are enabled or disabled in the BIOS?
 Are there performance issues (bugs) with this processor model? Are they listed in the processor errata sheet?
 Are there performance issues (bugs) with this BIOS firmware version?
 Are there software-imposed CPU usage limits (resource controls) present? What are they


## Priority Tuning
Unix has always provided a nice() system call for adjusting process priority,which sets a nice-ness value. Positive nice values result in lower process priority
(nicer), and negative values—which can be set only by the superuser (root)—result in higher priority. A nice(1) command became available to launch programs with
nice values, and a renice(1M) command was later added (in BSD) to adjust the nice value of already running processes. 

 ###### The value of 16 is recommended to users who wish to execute long-running programs without flak from the administration
  
 ###### renice -n 16 -p 29457

## Resource Controls

The operating system may provide fine-grained controls for allocating CPU cycles to processes or groups of processes. These may include fixed limits for CPU utilization and shares for a more flexible approach—allowing idle CPU cycles to be consumed based on a share value. How these work is implementation-specific and
discussed in Section 6.8, Tuning.

##### CPU Binding
Another way to tune CPU performance involves binding processes and threads to individual CPUs, or collections of CPUs. This can increase CPU cache warmth for
the process, improving its memory I/O performance. For NUMA systems it also improves memory locality, also improving performance.
There are generally two ways this is performed:

 Process binding: configuring a process to run only on a single CPU, or only on one CPU from a defined set.
 Exclusive CPU sets: partitioning a set of CPUs that can be used only by the process(es) assigned to them. This can improve CPU cache further, as when the process is idle other processes cannot use the CPUs, leaving the caches warm.


## Micro-Benchmarking
      There are various tools for CPU micro-benchmarking, which typically measure the time taken to perform a simple operation many times. 


 The operation may be based on the following:
 
 <b> CPU instructions</b> : integer arithmetic, floating-point operations, memory loads and stores, branch and other instruction   
 
 <b> Memory access</b> : to investigate latency of different CPU caches and main memory throughput
 
 <b> Higher-level languages </b> : similar to CPU instruction testing, but written in a higher-level interpreted or compiled language
 
 
 1. Determine the target user population or application request rate.
 
 2.  Express CPU usage per user or per request. For existing systems, CPU usage can be monitored with the current user count or request rate. For future systems, load generation tools can simulate users so that CPU usage can be measured.
 
 3.  Extrapolate users or requests when the CPU resources reach 100% utilization. This provides the theoretical limit for the system.
 
 Operating system operations testing system library and system call functions that are CPU-bound, such as getpid() and process creation       
 
 
   
### vmstat 8

procs -----------memory---------- ---swap--     -----io----      -system--                ------cpu-----

 |r   b   swpd    free      buff     cache       si    so     bi      bo    in     cs us sy id wa  st|
 |-|-0--|-------|------|-----------|-------|---------|--|-------|-------|--------|--|--|--|--|--|----| 
 0   0      0   2349620    69624    772432      0     0     108      7     68     159  2  1 95  3  0
  
 0   0      0   2349872    69632    772432      0     0       0      15    758    1406  0  1 99  0  0
 
 0   0      0   2349872    69632    772432      0     0       0      0      81    154  0  0 100  0  0
 
 
 The first line of output is the summary-since-boot, with the exception of r on Linux—which begins by showing current values. The columns are
   r: run-queue length—the total number of runnable threads (see below)
  us: user-time
  sy: system-time (kernel)
  id: idle
  wa: wait I/O, which measures CPU idle when threads are blocked on disk I/O
  st: stolen (not shown in the output), which for virtualized environments
  
shows CPU time spent servicing other tenants

### mpstat
The multiprocessor statistics tool, mpstat, can report statistics per CPU. Here is some example output from the Linux version:
       
       ###  mpstat -P ALL 1
                  
       |Average: |     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle |
       |-------- |---------|-------|------|-------|-------|--------|------|-------|-------|--------|------| 
        Average:     all    2.50    0.00    0.81    0.69    0.00    0.00    0.00    0.00    0.00   96.00
        Average:       0    3.23    0.00    1.00    0.25    0.00    0.00    0.00    0.00    0.00   95.52
        Average:       1    3.25    0.00    0.50    0.00    0.00    0.00    0.00    0.00    0.00   96.25
        Average:       2    2.26    0.00    1.00    2.51    0.00    0.00    0.00    0.00    0.00   94.24
        Average:       3    1.25    0.00    0.75    0.00    0.00    0.00    0.00    0.00    0.00   97.99

  
 
