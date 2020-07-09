#

If you don’t have perf on the system, easiest way you can do is:

$sudo apt-get update

$sudo apt-get install linux-tools-common linux-tools-generic linux-tools-`uname -r`

or $sudo apt-get install linux-tools-common linux-tools-generic

If you kernel version and linux-tool-generic version are same, you may be done here. However, most of time, the current update version may not be the same as what required. When you check $perf --version, you may get WARNING: perf not found for kernel and you may need to install the following packages for this specific kernel: … ,
where the right version is required.

# sudo apt install linux-tools-$(uname -r) linux-tools-generic
# which perf


