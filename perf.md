#

If you don’t have perf on the system, easiest way you can do is:

$sudo apt-get update

$sudo apt-get install linux-tools-common linux-tools-generic linux-tools-`uname -r`

or $sudo apt-get install linux-tools-common linux-tools-generic

If you kernel version and linux-tool-generic version are same, you may be done here. However, most of time, the current update version may not be the same as what required. When you check $perf --version, you may get WARNING: perf not found for kernel and you may need to install the following packages for this specific kernel: … ,
where the right version is required.

### sudo apt install linux-tools-$(uname -r) linux-tools-generic
### which perf

## sudo perf stat -a sleep 10

 Performance counter stats for 'system wide':

         40,104.14 msec cpu-clock                 #    4.001 CPUs utilized          
             7,502      context-switches          #    0.187 K/sec                  
               301      cpu-migrations            #    0.008 K/sec                  
               140      page-faults               #    0.003 K/sec                  
    1,93,53,31,749      cycles                    #    0.048 GHz                      (83.32%)
    2,29,23,38,976      stalled-cycles-frontend   #  118.45% frontend cycles idle     (83.32%)
    2,09,25,35,644      stalled-cycles-backend    #  108.12% backend cycles idle      (66.67%)
      37,84,77,853      instructions              #    0.20  insn per cycle         
                                                  #    6.06  stalled cycles per insn  (83.34%)
       7,68,27,602      branches                  #    1.916 M/sec                    (83.37%)
         84,08,740      branch-misses             #   10.94% of all branches          (83.32%)

      10.024048257 seconds time elapsed
              
   
The key metric here is instructions per cycle (insns per cycle: IPC), which shows on average how many instructions we were completed for each CPU clock cycle. The higher, the better (a simplification). The above example of 0.78 sounds not bad (78% busy?) until you realize that this processor's top speed is an IPC of 4.0. This is also known as 4-wide, referring to the instruction fetch/decode path. Which means, the CPU can retire (complete) four instructions with every clock cycle. So an IPC of 0.78 on a 4-wide system, means the CPUs are running at 19.5% their top speed. Newer Intel processors may move to 5-wide.

   perf record -F 99 -p 13204 -g -- sleep 30
   
   ##  For Hardware counter for each instruction use tiptop 
    
     sudo apt install tipto
     
     
     Conclusion

CPU utilization has become a deeply misleading metric: it includes cycles waiting on main memory, which can dominate modern workloads. Perhaps %CPU should be renamed to %CYC, short for cycles. You can figure out what %CPU really means by using additional metrics, including instructions per cycle (IPC). An IPC < 1.0 likely means memory bound, and an IPC > 1.0 likely means instruction bound. I covered IPC in my previous post, including an introduction to the Performance Monitoring Counters (PMCs) needed to measure it.

Performance monitoring products that show %CPU – which is all of them – should also show PMC metrics to explain what that means, and not mislead the end user. For example, they can show %CPU with IPC, and/or instruction-retired cycles vs stalled cycles. Armed with these metrics, developers and operators can choose how to better tune their applications and systems.

# Measuring IPC

Instructions-per-cycle (IPC) is a good starting point for PMC analysis, and is measured by counting the instruction count and cycle count PMCs. (On some systems it is shown as its invert, cycles-per-instruction, CPI.)

## IPC is like miles-per-gallon for CPUs: how much bang for your buck. The resource here isn't gallons of gasoline but CPU cycles, and the result isn't miles traveled but instructions retired (ie, completed). The more instructions you can complete with your fixed cycles resource, the better.

In the interest of keeping this short, I'll gloss over IPC caveats. There are situations where it can be misleading, like an increase of IPC because your program suffers more spin lock contention, and those spin instructions happen to be very fast. Just like MPG can be misleading, as it can be influenced by the route driven, not just the car's own characteristics.

# For every resources check 
 1) Utilization 
 2) Saturation
 3) Errors
 Including Busses and interconnects 
 Check the metrics for the same

### sudo perf record -e block:block_rq_issue -ag


### sudo perf report -n stdio





