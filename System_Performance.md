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