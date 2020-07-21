
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
   
### ls -F /proc/28712
attr/
cpuset
auxv
cwd@
cgroup
environ
clear_refs
exe@
cmdline
fd/
coredump_filter fdinfo/
io
latency
limits
loginuid
maps
mem
mountinfo
mounts
mountstats
net/
numa_maps
oom_adj
oom_score
pagemap
personality
root@
sched
schedstat
sessionid
smaps
stack
stat
statm
status
syscall
task/
wchan
The exact list of files available depends on the kernel version and CONFIG
options.
Those related to per-process performance observability include
 limits: in-effect resource limits
 maps: mapped memory regions
 sched: various CPU scheduler statistics
 schedstat: CPU runtime, latency, and time slices
 smaps: mapped memory regions with usage statistics
 stat: process status and statistics, including total CPU and memory usage
 statm: memory usage summary in units of pages
 status: stat and statm information, human-readable
 task: directory of per-task statistic
