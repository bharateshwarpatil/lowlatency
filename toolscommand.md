### mpstat

Linux 5.4.0-26-generic (Notebook-PC) 	18/07/20 	_x86_64_	(4 CPU)

12:38:33 PM IST  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
12:38:33 PM IST  all    8.20    0.21    1.73    2.92    0.00    0.05    0.00    0.00    0.00   86.87
 


-A
This option is equivalent to specifying -I ALL -u -P ALL
-I { SUM | CPU | ALL }
Report interrupts statistics.
With the SUM keyword, the mpstat command reports the total number of interrupts per processor. The following values are displayed:

CPU

Processor number. The keyword all indicates that statistics are calculated as averages among all processors.
intr/s

Show the total number of interrupts received per second by the CPU or CPUs.
With the CPU keyword, the number of each individual interrupt received per second by the CPU or CPUs is displayed.

The ALL keyword is equivalent to specifying all the keywords above and therefore all the interrupts statistics are displayed.

-P { cpu [,...] | ON | ALL }
Indicate the processor number for which statistics are to be reported. cpu is the processor number. Note that processor 0 is the first processor. The ON keyword indicates that statistics are to be reported for every online processor, whereas the ALL keyword indicates that statistics are to be reported for all processors.
-u
Report CPU utilization. The following values are displayed:
CPU

Processor number. The keyword all indicates that statistics are calculated as averages among all processors.
%usr

Show the percentage of CPU utilization that occurred while executing at the user level (application).
%nice

Show the percentage of CPU utilization that occurred while executing at the user level with nice priority.
%sys

Show the percentage of CPU utilization that occurred while executing at the system level (kernel). Note that this does not include time spent servicing hardware and software interrupts.
%iowait

Show the percentage of time that the CPU or CPUs were idle during which the system had an outstanding disk I/O request.
%irq

Show the percentage of time spent by the CPU or CPUs to service hardware interrupts.
%soft

Show the percentage of time spent by the CPU or CPUs to service software interrupts.
%steal

Show the percentage of time spent in involuntary wait by the virtual CPU or CPUs while the hypervisor was servicing another virtual processor.
%guest

Show the percentage of time spent by the CPU or CPUs to run a virtual processor.
%idle

Show the percentage of time that the CPU or CPUs were idle and the system did not have an outstanding disk I/O request.
Note: On SMP machines a processor that does not have any activity at all is a disabled (offline) processor.

-V
