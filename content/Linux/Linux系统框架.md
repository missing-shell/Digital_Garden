- 应用程序（裸机）--> 硬件（Haredare）
- 应用程序 --> [操作系统通用代码-->设备驱动（Linux内核）] --> 硬件（Haredare）
### 应用
- 关注业务逻辑
- 应用程序通过**系统调用**来使用内核资源
### 驱动
- 关注硬件特性
- 驱动是`Linux`内核的一部分，驱动的架构越来越复杂，目的是为了应用层需要做的事情越来越少
## 状态
内核态和用户态不仅是软件上的抽象，**ARM**处理器本身在硬件上就支持这两种状态
**ARM**处理器工作模式
 - 用户模式
 - 系统模式
 - 中断模式
### 用户态
- 不能直接访问硬件资源，直接访问会触发~~异常中断~~ ？？？？？？？？？？
- 普通函数调用由函数库或用户自己提供，运行在用户态
### 内核态
- 系统调用由操作系统核心提供，运行于内核态
- **系统调用**在`ARM`系统中一般用**软中断**的方式实现

