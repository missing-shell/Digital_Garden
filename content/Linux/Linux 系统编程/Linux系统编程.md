- 基于`Linux`内核的应用程序的设计
- 基于`Linux`内核的`APP`程序设计
- `Linux`系统下的高级编程，**介于应用层和驱动层**
---
#### 三种`APP`开发
- `Linux`内核提供系统调用
- `Linux`内核->`APP`
- `Linux`内核->`QT`图形库->`APP`
- `Linux`内核->`Andriod`系统->`APP`
#### 交叉编译
- 在主机上编译程序，通过串口或网线将二进制文件发送到目标机，在目标机上运行编译的二进制文件
#### 应用程序调试
- `U`盘或者`TF`卡拷贝
- **网络文件系统**：`NFS(Network File Sytem)`
- `GDB`可设置断点和调试，
- `strace` 拦截和记录系统调用及接收的信号
- `valgind` 检测内存泄漏