---
layout: post
published: true
category: worklog
title: 内存使用检查
---
console-kit-daemon is consuming large amounts of memory
===
用户报说console-kit-daemon进程使用的内存过多，超过100M

	Chg   PID Name                        VmStk       VmData       VmSize      VmPeak
	+  11842 console-kit-dae            136 kB    135516 kB    164116 kB    227340 kB

检查下了我当前 SP2 上 console-kit-daemon 的使用的统计信息，并查了下proc.txt文档。

	# cat /proc/1748/status
	Pid:    1748
	FDSize:    64
	VmPeak:      174648 kB     # 使用虚拟内存的最大值(peak virtual memory size)
	VmSize:      109112 kB     # total program size
	VmLck:           0 kB      # locked memory size
	VmHWM:        2748 kB      # peak resident set size ("high water mark")
	VmRSS:        1956 kB      # size of memory portions
	VmData:       78376 kB     # size of data, stack, and text segments
	VmStk:         136 kB      # size of data, stack, and text segments
	VmExe:         132 kB      # size of text segment
	VmLib:        3744 kB      # size of shared library code
	VmPTE:          88 kB      # size of page table entries
	VmSwap:         772 kB
	Threads:    64             # ???

之后用各种工具分析了一大通，为什么 vmData 和 VmSize 使用这么多内存？

经过Yifan同学的帮忙，注意到线程数过多，然后搜到了相应的bug。

<https://bugs.freedesktop.org/show_bug.cgi?id=17720>

关于Patch
===
什么是 VT_WAITEVENT?

在 2.6.32 以后的内核，支持了新的 ioctl 请求 VT_WAITEVENT
能够监视 vt 切换并返回新的 vt 号。

什么是VT?

在内核的drivers/tty/vt/目录下有vt的实现，看了下drivers/tty/Kconfig 中对 VT 的解释

VT 就是虚拟终端(Virtual terminal)，也可叫做虚拟控制台(virtual consoles)，之所以称为
虚拟是因为你可以在一个真实的物理终端上运行多个虚拟终端。
这相当有用，比如一个虚拟终端用于收集系统日志，一个用于用户使用文本界面的session，
一个用于 X 图形界面的session。在不同的终端用 Alt + <Fn> 来切换。

如果禁用了VT模块，那么你只能从串口或网络连接来登录系统，嵌入系统经常会禁用它以节省
资源。

为什么创建了64个线程？

这个问题是怎么解决的？

常用工具
===
ps 显示线程
	ps -eLf 

显示当前内存大小: 
	free -m |grep "Mem" | awk '{print $2}'

内存使用统计，只统计了rss字段:
	ps -eo fname,rss|awk '{arr[$1]+=$2} END {for (i in arr) {print i,arr[i]}}'|sort -k2 -nr

	rss        RSS      resident set size, the non-swapped physical memory that a task has used (in kiloBytes). (alias rssize, rsz)
	fname      COMMAND  first 8 bytes of the base name of the process's executable file. The output in this column may contain spaces.

pmap
====

	$ pmap -d 31311
	31311: console-kit-dae
	START               SIZE     RSS     PSS   DIRTY    SWAP PERM OFFSET   DEVICE
	MAPPING
	0000000000400000    132K    108K    108K      0K      0K r-xp 0000000000000000
	08:06  /usr/sbin/console-kit-daemon
	0000000000620000      4K      4K      4K      4K      0K r--p 0000000000020000
	08:06  /usr/sbin/console-kit-daemon
	0000000000621000      4K      4K      4K      4K      0K rw-p 0000000000021000
	08:06  /usr/sbin/console-kit-daemon
	0000000000622000    132K    108K    108K    108K      0K rw-p 0000000000000000
	00:00  [heap]
	0000000000643000    132K     12K     12K     12K      0K rw-p 0000000000000000
	00:00  [heap]
	00007fa680000000    132K      4K      4K      4K      0K rw-p 0000000000000000
	00:00  [anon]
	00007fa680021000  65404K      0K      0K      0K      0K ---p 0000000000000000
	00:00  [anon]
	.....
	Total:            98712K   2656K   1262K   1080K      0K

valgrind
====
valgrind's memcheck and callgrind 

	$ valgrind  --leak-check=full --log-file=ckd  /usr/sbin/console-kit-daemon
	$ valgrind --tool=callgrind /usr/sbin/console-kit-daemon
	$ callgrind_annotate callgrind.out.12195
        $ kcachegrind callgrind.out.12195 # 图形化的工具
	$ valgrind --tool=massif --pages-as-heap=yes --depth=50 /usr/sbin/console-kit-daemon --no-daemon
	massif.out.12227

参考
===
pmap : 理解linux的进程内存占用
<http://blog.hellosa.org/2010/02/26/pmap-process-memory.html>
