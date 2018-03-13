# 进程与线程的区别

进程是操作系统中分配资源的基本单位, 而线程则为运行与调度的基本单位.  
其中资源的所有权包括程序(program text), 数据(data), 文件(open file)等  
则在同一进程中的线程共享相同的地址空间, 因此线程间通信很方便, 而进程间通信(IPC)则较困难  

## 进程与线程的区别_名词解释来源

进程与线程都是一个时间段的描述, 是CPU工作时间段的描述, 只不过颗粒大小不同.  
进程就是包换上下文切换的程序执行时间总和 = CPU加载上下文+CPU执行+CPU保存上下文  
线程是共享了进程的上下文环境，的更为细小的CPU时间段。

### CPU与其它资源(上下文)

CPU的任务都是一个一个轮流执行的, 执行一段程序代码, 实现一个功能的过程时, 当得到CPU时, 相关的其它资源也必然的到位的了.  
然后除了CPU之外的所有资源就构成了这个程序的执行环境, 也即我们定义的程序上下文.  
当程序执行完, 或分配给它的CPU执行时间消耗完后, 需被切换出去, 等待下一次CPU对它执行时, 会做最后一步的执行工作为保存此时的程序上下文.  


则结合起来, 在CPU看来, 具体的工作流程为:  
先加载程序A的上下文, 然后执行A, 保存程序A的上下文; 调入下一个要执行的程序B的程序上下文, 然后执行B, 然后再保存程序B的上下文.

## 创建进程与创建线程的开销比较

### per process items

Address space, Global variables, Open files, Child processes, Pending alarms, Signals and signal handlers, Accounting information  
地址空间(不是真实内存的排布, 虚拟内存地址, 是由很多个页组成)
全局变量, 操作系统的文件描述符或文件句柄(stin, stdout, stderr)
子进程, pending(即将发生的) alarms(警告), 
异常以及异常处理, Accounting(记账) 信息

### per thread items

Program Counter, Registers, Stack, State(线程状态: 堵塞, 等待)
