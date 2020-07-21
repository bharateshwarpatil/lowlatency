
#### These tools examine system-wide activity in the context of system software or hardware resources, using kernel counters. Examples are
   ### vmstat: virtual and physical memory statistics, system-wide
   ### mpstat: per-CPU usage
   ### iostat: per-disk I/O usage, reported from the block device interface
   ### netstat: network interface statistics, TCP/IP stack statistics, and some per-connection statObservability Sourcesistics
   ### sar: various statistics; can also archive them for historical reportingType Linux Solaris


## Source

Per-process counters:  /proc /proc, 
System-wide counters:  /proc, /sys 
Device driver and debug info:  /sys 
Per-process tracing:  ptrace, uprobes procfs, 
CPU performance counters:  perf_event 
Network tracing:  libpcap libdlpi, 
Per-thread latency metrics:  delay accounting
System-wide tracing:  tracepoints, kprobes, ftrace 



   
 ###  /proc
This is a file system interface for kernel statistics. /proc contains a number of directories, where each directory is named after the process ID for the process it
represents. These directories contain a number of files containing information and statistics about each process, mapped from kernel data structures. On Linux, there are additional files in /proc for system-wide statistics. /proc is dynamically created by the kernel and is not
   
