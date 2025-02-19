- [ ] 什么是posix
- [ ] 为什么通过apt-get去下载依赖库，有没有其他方式
## Linux 内核
- [ ] [[Linux内核#用户空间和内核通信的方式|用户态和内核态的转换]]
- [ ] [[Linux内核#用户态和内核态的区别及区分原因？|用户态和内核态的区别，以及系统为什么要分用户态与内核态]]
### 进程管理
- [ ] [[03_多线程同步#同步|常用线程同步方式]]
- [ ] [[01_进程调度算法#调度策略|如果现在多个线程，怎么确定哪一个会先被执行？]]
- [ ] 如果现在是一个单核的CPU，那么多个线程是按什么顺序去运行的？也就是线程的系统调度？
- [ ] [[01_进程调度算法#调度器管理器|Linux有哪些调度方式，cfs的调度原理]]
- [ ] [[01_进程与线程#线程与进程的区别|进程和线程的区别]]
- [ ] [[01_进程与线程#线程与进程的区别|进程和线程通信方式有什么不同]]
- [ ] [[01_进程调度算法#Linux进程调度|进程调度算法由操作系统内核哪个部分完成]] `SDcheduler`
- [ ] 进程调度，内核里面调度优先级怎么实现的。
- [ ] [[02_进程间通信#多进程、多线程同步(通讯)方法⭐⭐⭐⭐|线程、进程之间哪些通信方式?]]
- [ ] [[02_内存堆栈管理#用户栈与内核栈的切换|压栈出栈会导致什么问题？]]
- [ ] [[02_内存堆栈管理#默认栈空间|一般，一个线程栈或者一个进程栈的大小是多少？]]
- [ ] [[02_内存堆栈管理#堆内存管理|发生栈溢出，程序会怎么样？]]
- [ ] [[01_进程与线程#进程创建|如何创建新的进程]]
- [ ] [[01_进程与线程#`execve()`|如何启动子进程]] #todo
- [ ] [[01_进程与线程#线程与进程的区别|为什么说进程比线程开销大]]
- [ ] [[01_进程与线程#线程与进程的区别|为什么说进程是分配的最小单元，线程是调度的最小单元]]
- [ ] [[01_进程调度算法#时间片轮转（Round Robin, RR）|时间片轮转是如何调度的]]
- [ ] [[01_进程与线程#如何降低线程崩溃的影响|8个线程 有一个崩溃了 如何保证对其他线程没有影响]]
- [ ] [[01_进程与线程#线程切换详细流程|线程上下文切换的 会涉及到哪些]]
- [ ] [[01_进程与线程#线程切换详细流程|任务切换需要做些什么]]
- [ ] 普通进程，一个120 一个130运行相同的物理时间谁的虚拟时间更大
- [ ] [[02_内存堆栈管理#多进程共享动态库|两个程序加载相同的库文件，物理地址相同，虚拟地址相同么]]
- [ ] [[01_进程与线程#线程池|线程池]]
- [ ] [[OS#孤儿进程、僵尸进程、守护进程的概念⭐⭐⭐|僵尸进程、孤儿进程]]
- [ ] [[OS#正确处理僵尸进程的方法⭐⭐|怎么看系统上的僵尸进程，看到后怎么去掉僵尸进程]]
- [ ] [[01_进程与线程#创建线程|如何创建线程]]
- [ ] [[OS#五状态转换⭐⭐⭐⭐|进程的常见状态]]
- [ ] [[01_进程与线程#进程创建|创建进程的方法]]
- [ ] [[01_进程调度算法#抢占式调度|进程调度算法有哪些？]]
#### 进程间通信
- [ ] [[03_多线程同步#同步|常用线程同步方式]]
- [ ] [[05_死锁#死锁预防|多线程访问同一个数组如何避免死锁？]]
- [ ] [[04_线程间通信#自旋锁（Spinlock）|自旋锁实现原理、大概讲了一下汇编部分]]
- [ ] [[05_死锁#概念|死锁原因、解决办法、预防方法]]
- [ ] [[02_进程间通信#定义|进程通信方式]]
- [ ] 内核数据拷贝
- [ ] [[02_进程间通信#System V IPC#IPC通信方式]]
- [ ] [[02_内存堆栈管理#通过缺页中断填充页表|页中断的机制]]
- [ ] [[02_页面置换算法|页面的替换算法有哪些？]]
- [ ] [[02_进程间通信#共享内存|为什么使用共享内存?]]
- [ ] [[04_线程间通信#互斥锁（`mutex`）|互斥锁和信号量 区别]] #todo
- [ ] [[04_线程间通信|锁的底层实现]]
- [ ] [[02_进程间通信#共享内存如何实现数据同步|共享内存如何实现数据同步]]
- [ ] [[03_多线程同步#原子操作|原子操作的理解]]
- [ ] [[03_多线程同步|同步和异步的优点和缺点]]
- [ ] [[02_进程间通信#Socket|Linux进程间通信Socket作用场景]]
- [ ] [[01_进程与线程#进程、线程优缺点|多线程的使用]]
- [ ] [[02_进程间通信#管道 ⭐⭐⭐|管道能够承载的最大传输数据量是多少]]
- [ ] [[02_进程间通信#管道破裂|管道破裂的解决方式]]
- [ ] [[02_进程间通信#System V IPC|进程通讯方式，共享内存和消息队列的区别]]
- [ ] [[02_进程间通信#管道优势|管道的优势]]
- [ ] [[02_进程间通信#Socket|同进程多线程，可以socket通讯吗]]
### 内存管理
- [ ] [[05_MMU#虚拟地址-->物理地址|虚拟地址和物理地址的关系]]
- [ ] [[02_内存堆栈管理#内存管理的进程和硬件背景|页表]]
- [ ] [[02_内存堆栈管理#通过缺页中断填充页表|页表和缺页中断]]
- [ ] 发生页面故障时，操作系统会怎么处理？
- [ ] [[02_内存堆栈管理#常见的内存错误及检测|Linux段错误的常见原因及排查方法]]
- [ ] [[02_内存堆栈管理#内存泄漏|内存泄漏的成因及预防措施]]
- [ ] [[02_内存堆栈管理#常见的内存错误及检测|内存没有释放的影响及堆内存检查]]
- [ ] [[02_内存堆栈管理#内存踩踏|内存践踏的现象]]
- [ ] `Valgrind` 的*底层实现原理*，或者是底层思想，如何检查出来内存泄露？ 可以检查哪些错误？你检查过哪些错误？
- [ ] [[01_虚拟内存#虚拟内存与物理内存的区别|虚拟地址和物理地址的区别？]]
- [ ] [[02_内存堆栈管理#Linux堆内存管理|malloc 1g和 10m的时间有差别吗？]]
- [ ] [[02_内存堆栈管理#堆内存与栈的区别|堆和栈有什么区别？]]
- [ ] [[05_MMU#虚拟地址-->物理地址|虚拟地址到物理地址]]
- [ ] [[05_MMU#物理地址-->虚拟地址|反过来物理地址到虚拟地址]]
- [ ] [[02_内存堆栈管理#mmap映射|mmap底层怎么实现的，怎么分配内存的]]
- [ ] [[CPU#Cache|为什么要用cache，从原理以及缓存命中等角度考虑。]]
- [ ] [[CPU#内存与Cache的一致性|cache相关，如何实现缓存一致性]]
- [ ] [[CPU#Cache|CPU架构，cache，怎么完成数据读取]]
- [ ] [[05_MMU#虚拟地址-->物理地址|MMU如何映射？]]
- [ ] [[01_虚拟内存#虚拟内存定义|虚拟内存的功能是什么？]]
- [ ] 向物理内存4G的机器申请 16G内存可以么 32位可以 64位不可以
- [ ] [[05_MMU#虚拟地址-->物理地址|如何从虚拟地址转换为物理地址 软件上 硬件上]]
- [ ] [[01_虚拟内存#内存分页|如何实现非连续分区的内存分配]]
- [ ] [[01_虚拟内存#内存分页|内存分页是怎么分的]]
- [ ] Linux 怎么避免内存碎片
- [ ] [[05_MMU#虚拟地址-->物理地址|虚拟地址和物理地址之间交互，页表，上下文]]
### 网络I/O
- [ ] [[03_IO多路复用#零拷贝|零拷贝技术原理]]
- [x] [[03_IO多路复用#epoll|epoll 为什么叫io 多路复用，讲解一下 io 多路复用]]
- [x] [[03_IO多路复用#select/poll|IO多路复用select和poll的区别]]
- [x] [[03_IO多路复用#五种I/O模式|阻塞式I/O与非阻塞式I/O]]
- [x] [[03_IO多路复用#select->poll->epoll|IO多路复用实现与细节、epoll好处]]
- [x] [[03_IO多路复用#文件描述符数量限制|epoll支持的最大连接数]]
### 文件系统
- [ ] [[01_文件系统#vfs文件系统|对VFS的框架有了解么？]]
## Linux驱动
- [ ] makefile改过吗，如何修改
- [ ] Makefile最终是使用什么把可执行文件编译出来的
- [ ] Makefile添加依赖库怎么操作
- [ ] 编译一个hello.c具体怎么写Makefile
- [ ] make的时候执行那一条命令是怎么找的。冒号后面写指令有什么要求和限制
- [ ] 板子是如何烧录内核的
- [ ] makefile是干什么的，规则是什么样的
- [ ] makefile中，如果是头文件有修改，源文件不修改，make会有什么结果
### 系统调用
- [ ] [[Linux内核#系统调用与普通函数调用的区别|讲一下系统调用，以及为什么要系统调用]]
- [ ] [[Linux内核#系统调用read/write的内核处理流程|系统调用流程？]]
- [ ] [[Linux内核#系统调用参数传递|系统调用传递参数 用什么传递 如果不用寄存器传参数用什么传参数]]
- [ ] [[Linux内核#内核中系统调用的处理|操作系统系统调用的过程 详细介绍]]
### 中断子系统
- [ ] [[Linux中断#硬中断与软中断|软中断和硬中断的区别]]
- [ ] [[Linux中断#中断上下文|中断上下文]]
- [ ] [[Linux中断#中断上下部|上半部和下半部使用场景]]
- [ ] [[Linux中断#中断处理大致流程|中断流程]]
- [ ] [[Linux中断#中断中的资源共享方式|中断里面用什么方式实现资源共享]]
- [ ] [[Linux中断#中断中的资源共享方式|中断嵌套可以使用自旋锁嘛]]
- [ ] [[Softirq, Tasklets and Workqueues#中断上下文与下半部机制|中断为什么不能用互斥锁]] `中断处理程序不允许休眠`
- [ ] [[Linux中断#中断上下部|中断 为什么上下部]]
- [ ] [[Linux中断#概念|Linux中断？]]
- [ ] 中断是什么，分为哪些，上半部下半部详细讲讲，下半部如何处理
- [ ] [[Softirq, Tasklets and Workqueues#中断下半部实现方式|除了tasklet和工作队列，还能用什么方式实现中断下半部]]
- [ ] [[Softirq, Tasklets and Workqueues#中断上下文与下半部机制|中断中禁止使用哪些？中断上半部可以使用 kmalloc嘛，sleep呢]] `中断上半部禁止可能导致休眠的操作`
### 内存子系统
- [ ] [[01_内存子系统#内存分配API|常见的内存分配api有哪些，kmallloc，vmalloc,kmap这些的区别，底层怎么实现的。]]
- [ ] [[02_buddy子系统#伙伴系统的底层实现|说一下内存管理，伙伴系统的底层]]
- [ ] [[02_buddy子系统#4G内存分配流程|如果4g内存分配一页，伙伴系统流程是什么样的]]
- [ ] [[02_buddy子系统#伙伴系统概述|伙伴系统最小支持的内存大小是多少]]
- [ ] [[01_内存子系统#内存分配API|内存分配函数，kmalloc和vmalloc，地址连续吗]]
### 模块
- [ ] [[02_内核模块#实现驱动模块的目的|为什么要实现驱动模块？是什么目的？]]
- [ ] [[02_内核模块#驱动模块初始化|驱动模块初始化]]
- [ ] [[02_内核模块#驱动模块的读写操作|驱动模块读写]]
- [ ] 驱动的类结构有哪些函数，函数分别是什么时候调用的，哪些是内核调用的，哪些是用户调用的
- [ ] 字符驱动注册的过程是什么样的
- [ ] 什么是字符设备、块设备
- [ ] 设备和驱动是如何匹配的
- [ ] 设备树存在的意义
- [ ] dtb 的功能，有什么缺点
- [ ] Probe 函数调用
- [ ] 驱动在Linux是如何匹配的
- [ ] 总线的作用 ~~字符设备驱动~~
- [ ] 在已有驱动的情况下，如何添加新的设备（不使用设备树，不重启，不使用应用层工具）
- [ ] 介绍了设备树的软件框架呈现
- [ ] 驱动分类
- [ ] 字符设备驱动基本框架
- [ ] 加载驱动、卸载驱动、查看驱动
- [ ] dev是由谁来创建的
- [ ] 设备树，怎么申请空内存给设备
- [ ] 应用层和驱动是怎么交互的
- [ ] 应用层不用read怎么拿到驱动层的数据，通知或触发形式
- [ ] 信号在驱动层触发应用层可以捕获到吗
- [ ] 什么是input子系统，识别到事件之后是怎么给应用层的
- [ ] 设备树总线模型，以及匹配的过程
- [ ] 设备树里面主要的一些节点以及作用
- [ ] 一个驱动里面需要哪些设备树信息
- [ ] gpio驱动怎么写
- [ ] 怎么写的驱动，接口
- [ ] Linux驱动和应用如何交互
- [ ] 为什么两个驱动要交互，他们交互的流程
- [ ] 回调需要注意什么
- [ ] 缓冲区，为什么不用cache，如何解决内存不一致，Linux如何绕过cache
- [ ] 设备树如何加载进内核的
- [ ] copy_from_user怎么实现的，为什么要拷贝一次，不用直接映射
- [ ] 现在要你来开发一个led，实现流水灯你会怎么做？
	（mark一下，这个我感觉当时光顾着体现“驱动实现机制，应用实现策略”的思想了，脑子抽了答了那种最low的字符设备驱动写法，字符设备框架部分没啥问题，但是在硬件的处理上答得不太好，我答的是控制led，用ioremap去映射寄存器地址然后操控gpio，或者像是之前比较老的内核，用平台的函数去控制gpio。我后来一想其实新的内核，就是为了解决平台代码、硬件信息过多地嵌入驱动的问题，从而来实现彻底的软硬件分离。更恰当很符合linux思想的做法应该是，在设备树描述gpio，然后驱动用of函数得到硬件信息，用pinctrl子系统的接口控制gpio，这样写出来的led驱动才真正做到了软硬件分离。面试官还问我，应该说是提醒，用ioremap有什么缺点？？我当时确实是没想起来，可惜）
## Linux 启动流程
- [ ] Linux内核裁剪
- [ ] 镜像构造
- [ ] emmc启动和sd卡启动的区别
- [ ] 为什么需要Uboot，不能直接在内核里面把Uboot 的事情做了
- [ ] 说说linux的uboot移植和内核移植这部分你做了哪些工作？
- [ ] uboot相关功能、流程
## Linux 命令
- [ ] [[OS/Linux/Linux#查看进程运行状态的指令|查看Linux系统内存占用和进程信息]]
- [ ] 常见命令：ls mkdir cat grep，查看进程的命令
- [ ] top命令一般展示什么
- [ ] 说一下常用的Linux调试工具
- [ ] 有用过linux吗，介绍一下常用的指令
- [ ] shell命令查找某个目录及子目录的某个文件
- [ ] shell 如何大小写切换，如何将字符转为数字等等
- [ ] Linux 查找一个文件、查找一个字符串