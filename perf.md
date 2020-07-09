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
     
   
   
