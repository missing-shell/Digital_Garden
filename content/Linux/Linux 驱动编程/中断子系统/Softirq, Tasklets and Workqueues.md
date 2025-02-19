> 当中断发生时，操作系统必须确保以下步骤：

- 内核必须暂停当前进程的执行；（抢占当前任务）；
- 内核必须寻找中断的处理程序并转移控制权（执行中断处理程序）；
- 中断处理程序执行完毕后，被中断的进程才能恢复执行。
#### 中断向量表
每个中断处理程序的地址都保存在一个特殊的位置，这个位置被称为 `中断描述符表（Interrupt Descriptor Table）` 或者 `IDT`。处理器使用一个唯一的数字来识别中断和异常的类型，这个数字被称为 `中断标识码（vector number）`。一个中断标识码就是一个 `IDT` 的标识。中断标识码范围是有限的，从 `0` 到 `255`。

## 中断下半部实现方式
在 Linux 内核中，**中断下半部**（Bottom Half）是指用于处理中断的**延迟部分**。这些任务通常在*中断处理函数*的执行完成之后进行，以便释放中断上下文中的时间，避免长时间占用 CPU 时间。中断的下半部主要用于处理非紧急但需要处理的任务。
### 软中断 (SoftIRQ)
软中断是 Linux 中用于延迟中断处理的一个机制。它允许在**中断上下文**中完成一些高优先级的任务，在当前中断完成后稍后执行。软中断主要用于处理一些系统级别的事件，比如网络包的接收处理等。
#### 特点
- **执行方式**：软中断的处理是在中断返回后由*内核调度器*执行的。软中断可以看作是硬中断的延续，运行在内核中断上下文中。
- **并发性**：可以有多个软中断同时执行，内核会根据软中断的优先级和调度顺序来决定执行哪个软中断。
- **不可阻塞**：软中断运行时不能进行阻塞操作（比如睡眠）。它只能执行非阻塞的操作。
- **适用场景**：网络协议栈的处理、定时任务等。

#### 软中断机制的实现
- 软中断是通过在内核中设置 `softirq` 队列，处理完硬中断后，由内核调度去执行。
- `softirq` 被划分为若干个类型，每个类型处理不同的任务（如网络包接收、TIMER、块设备等）。

### Tasklet
`Tasklet`是基于软中断的机制，允许将中断处理函数中的某些非紧急任务推迟到稍后执行。与软中断不同，`tasklet` 的任务是在软中断上下文中执行的，但它的执行顺序是通过**优先级**来决定的。
#### 特点
- **执行方式**：`tasklet` 是由内核调度器在*软中断上下文*中异步执行的。
- **不可并发执行**：`tasklet` 在同一时间只会有一个实例执行，不会发生并发执行的问题。它们使用自旋锁保证不被并发执行。
- **优先级**：可以为每个 `tasklet` 设置优先级，以控制它们的执行顺序。
- **不可阻塞**：和软中断一样，`tasklet` 的处理函数不能执行阻塞操作。
### 工作队列 (Workqueues)
工作队列允许将任务推迟到**进程上下文**中执行，因此可以执行**阻塞操作**（例如内存分配、文件操作等）。
#### 特点
- **执行方式**：工作队列的任务由内核工作线程执行，这些工作线程运行在进程上下文中。
- **可阻塞**：与软中断和 `tasklet` 不同，工作队列的任务可以执行阻塞操作，因为它们在进程上下文中执行。
- **适用场景**：需要阻塞操作的任务，比如文件系统操作、网络数据包处理等。
### 中断上下文与下半部机制
- **硬中断上下文**：在硬中断处理函数中执行，不能进行阻塞操作，也不能进行进程调度（即不能睡眠）。
- **软中断上下文**：处理与硬中断相关的延迟任务，但仍然不能进行阻塞操作。软中断是高优先级的任务，可以并发执行。
- **进程上下文**：允许执行阻塞操作和进程调度。工作队列运行在进程上下文中，可以执行阻塞操作。
- **Tasklet 和工作队列的关系**：`Tasklet` 是软中断的一部分，可以理解为*轻量级*的延迟任务处理；工作队列则是为了需要更复杂操作（如阻塞）而引入的机制。
