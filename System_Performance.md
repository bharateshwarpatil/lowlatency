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


The -P ALL option was used to print the per-CPU report. By default, mpstat(1) prints only the system-wide summary line (all).
The columns are

 CPU: logical CPU ID, or all for summary

 %usr: user-time

 %nice: user-time for processes with a nice’d priority

 %sys: system-time (kernel)

 %iowait: I/O wait

 %irq: hardware interrupt CPU usage

 %soft: software interrupt CPU usage

 %steal: time spent servicing other tenants

 %guest: CPU time spent in guest virtual machines

 %idle: idle

Key columns are %usr, %sys, and %idle. These identify CPU usage per CPU and show the user-time/kernel-time ratio (see Section 6.3.7, User-Time/Kernel-Time).
This can also identify “hot” CPUs—those running at 100% utilization (%usr +%sys) while others are not—which can be caused by single-threaded application
workloads or device interrupt mapping.




## perf

Originally called Performance Counters for Linux (PCL), the perf(1) command has evolved and become a collection of tools for profiling and tracing, now called
Linux Performance Events (LPE). Each tool is selected as a subcommand. For example, perf stat executes the stat command, which provides CPC-based statistics. These commands are listed in the USAGE message, and a selection is reproduced here in Table 6.8

### System Profiling

perf(1) can be used to profile CPU call paths, summarizing where CPU time is spent in both kernel- and user-space. This is performed by the record command,
which captures samples at regular intervals to a perf.data file. A report command is then used to view the file.In the following example, all CPUs (-a) are sampled with call stacks (-g) at 997 Hz (-F 997) for 10 s (sleep 10). The --stdio option is used to print all the output, instead of operating in interactive mode


<b> perf record -a -g -F 997 sleep 10 </b> 
    
<b> perf report --stdio </b>



    
 <b> perf sched record sleep 10 </b> 

 <b>  perf sched latency </b>  



-----------------------------------------------------------------------------------------------------------------
  Task                  |   Runtime ms  | Switches | Average delay ms | Maximum delay ms | Maximum delay at       |
 -----------------------------------------------------------------------------------------------------------------
  Chrome_IOThread:2113  |      3.245 ms |       34 | avg:    0.085 ms | max:    0.731 ms | max at:   1758.127910 s
  Chrome_ChildIOT:(4)   |      7.140 ms |       50 | avg:    0.080 ms | max:    1.086 ms | max at:   1763.143356 s
  JS Helper:(4)         |      1.594 ms |       25 | avg:    0.077 ms | max:    0.778 ms | max at:   1757.090081 s
  chrome:(5)            |     93.762 ms |       72 | avg:    0.076 ms | max:    0.831 ms | max at:   1757.127820 s
  tracker-miner-f:1399  |     83.643 ms |       38 | avg:    0.061 ms | max:    0.738 ms | max at:   1754.464164 s
  gmain:(9)             |      4.097 ms |      107 | avg:    0.059 ms | max:    0.840 ms | max at:   1760.879317 s
  ServiceWorker t:3459  |      0.961 ms |        7 | avg:    0.057 ms | max:    0.179 ms | max at:   1761.128444 s
  ssh-agent:1548        |      0.016 ms |        1 | avg:    0.051 ms | max:    0.051 ms | max at:   1761.128131 s
  ThreadPoolForeg:(7)   |      2.981 ms |       37 | avg:    0.031 ms | max:    0.433 ms | max at:   1763.143852 s
  kworker/3:0-eve:31    |      1.682 ms |       47 | avg:    0.029 ms | max:    0.546 ms | max at:   1758.129023 s
  gnome-terminal-:3542  |     18.632 ms |      125 | avg:    0.020 ms | max:    0.106 ms | max at:   1755.534176 s
  kworker/u16:5-e:255   |      1.553 ms |       49 | avg:    0.019 ms | max:    0.304 ms | max at:   1756.269625 s
  Xorg:1454             |     28.249 ms |      255 | avg:    0.016 ms | max:    0.472 ms | max at:   1755.162585 s
  kworker/u16:2-i:3597  |      1.310 ms |       44 | avg:    0.015 ms | max:    0.351 ms | max at:   1760.469115 s
  gnome-shell:1679      |     63.657 ms |      165 | avg:    0.013 ms | max:    0.544 ms | max at:   1760.476801 s
  wpa_supplicant:762    |      0.035 ms |        1 | avg:    0.013 ms | max:    0.013 ms | max at:   1763.147825 s
  ksoftirqd/0:9         |      0.098 ms |       11 | avg:    0.013 ms | max:    0.042 ms | max at:   1759.280363 s
  gedit:3726            |      0.871 ms |       16 | avg:    0.010 ms | max:    0.016 ms | max at:   1756.745903 s
  ksoftirqd/1:18        |      0.216 ms |       29 | avg:    0.010 ms | max:    0.071 ms | max at:   1761.680079 s
  kworker/1:0-eve:3698  |      0.651 ms |       60 | avg:    0.009 ms | max:    0.027 ms | max at:   1757.036655 s
  InputThread:1472      |      2.810 ms |       43 | avg:    0.008 ms | max:    0.013 ms | max at:   1755.067619 s
  rcu_sched:10          |      1.029 ms |      132 | avg:    0.007 ms | max:    0.068 ms | max at:   1761.680073 s
  migration/2:23        |      0.000 ms |        3 | avg:    0.007 ms | max:    0.008 ms | max at:   1760.236192 s
  systemd-resolve:703   |      0.608 ms |        4 | avg:    0.007 ms | max:    0.017 ms | max at:   1763.143857 s
  migration/0:11        |      0.000 ms |        3 | avg:    0.007 ms | max:    0.007 ms | max at:   1756.232724 s
  migration/3:29        |      0.000 ms |        3 | avg:    0.006 ms | max:    0.007 ms | max at:   1764.239659 s
  rtkit-daemon:(2)      |      0.031 ms |        2 | avg:    0.006 ms | max:    0.010 ms | max at:   1760.332193 s
  jbd2/sda5-8:288       |      0.140 ms |        3 | avg:    0.006 ms | max:    0.008 ms | max at:   1759.135891 s
  kerneloops:(2)        |      2.422 ms |        2 | avg:    0.006 ms | max:    0.006 ms | max at:   1759.124348 s
  migration/1:17        |      0.000 ms |        4 | avg:    0.005 ms | max:    0.009 ms | max at:   1756.236726 s
  kworker/2:1-mm_:3211  |      2.711 ms |       39 | avg:    0.005 ms | max:    0.040 ms | max at:   1763.147857 s
  sleep:3835            |      1.101 ms |        2 | avg:    0.005 ms | max:    0.006 ms | max at:   1754.453818 s
  thermald:817          |      0.363 ms |        3 | avg:    0.005 ms | max:    0.006 ms | max at:   1758.912654 s
  kworker/0:3-mm_:258   |      0.677 ms |       53 | avg:    0.005 ms | max:    0.011 ms | max at:   1757.100626 s
  GpuWatchdog:2216      |      0.091 ms |        1 | avg:    0.005 ms | max:    0.005 ms | max at:   1759.831514 s
  gvfs-afc-volume:1745  |      0.210 ms |       10 | avg:    0.005 ms | max:    0.012 ms | max at:   1762.311925 s
  kworker/1:1H-kb:286   |      0.232 ms |       13 | avg:    0.005 ms | max:    0.012 ms | max at:   1760.490611 s
  systemd:1             |      0.671 ms |       63 | avg:    0.004 ms | max:    0.017 ms | max at:   1761.693031 s
  perf:(2)              |     19.796 ms |       11 | avg:    0.004 ms | max:    0.006 ms | max at:   1758.249253 s
  java:(12)             |  19988.316 ms |     3489 | avg:    0.004 ms | max:    0.394 ms | max at:   1759.245167 s
  irqbalance:748        |      0.393 ms |        1 | avg:    0.004 ms | max:    0.004 ms | max at:   1758.136479 s
  kworker/0:1H-kb:260   |      0.092 ms |        7 | avg:    0.004 ms | max:    0.007 ms | max at:   1757.100619 s
  :3840:3840            |      0.102 ms |        1 | avg:    0.004 ms | max:    0.004 ms | max at:   1754.887729 s
  cron:713              |      0.075 ms |        1 | avg:    0.004 ms | max:    0.004 ms | max at:   1757.802634 s
  kworker/3:1H-ev:268   |      0.028 ms |        3 | avg:    0.003 ms | max:    0.004 ms | max at:   1763.496656 s
  :3838:3838            |      0.017 ms |        1 | avg:    0.003 ms | max:    0.003 ms | max at:   1754.818468 s
  pool-/usr/libex:(3)   |      0.126 ms |        3 | avg:    0.003 ms | max:    0.005 ms | max at:   1754.530476 s
  systemd-journal:329   |      0.089 ms |        1 | avg:    0.003 ms | max:    0.003 ms | max at:   1754.454122 s
  ksoftirqd/3:30        |      0.045 ms |        6 | avg:    0.003 ms | max:    0.003 ms | max at:   1756.236741 s
  kworker/2:1H-kb:259   |      0.039 ms |        3 | avg:    0.003 ms | max:    0.003 ms | max at:   1762.895441 s
  tracker-store:3827    |      0.025 ms |        1 | avg:    0.002 ms | max:    0.002 ms | max at:   1755.241849 s
  Chrome_SyncThre:2357  |      0.071 ms |        1 | avg:    0.002 ms | max:    0.002 ms | max at:   1757.487846 s
 -----------------------------------------------------------------------------------------------------------------
  TOTAL:                |  20336.702 ms |     5085 |
 ---------------------------------------------------




 
