- 如何构建C项目，包括不同的编译和链接阶段。
- 栈和堆是什么.
- 标准 C 库 `malloc()` 和 `free()` 函数。
- 全局变量的生命期一直到程序的结束
## 关键字

### typdef
> 为一种类型引入新的名字，而不是为变量分配空间。

- *对于现有类型*：`typedef 现有类型 新别名;`
- *对于函数指针*：`typedef void (*新别名)(int);`
- *对于结构体*：
```c
//在定义结构体的同时创建别名
typedef struct StructName {
    // 成员定义
} TypedefName;

//先定义结构体，然后创建别名
struct StructName {
    // 成员定义
};

typedef struct StructName TypedefName;
```

| 特性       | `typedef`               | `enum`                           | `#define`           |
| -------- | ----------------------- | -------------------------------- | ------------------- |
| *定义类型别名* | 是，为类型定义符号名称             | 否，定义枚举类型                         | 是，为类型和**数值**定义别名    |
| *作用域*    | *编译器*执行解释，有作用域限制        | 编译器执行解释，有作用域限制                   | *预编译器处理*，==无作用域限制== |
| *调试可见性*  | 可见                      | 可见                               | 不可见                 |
| *类型安全*   | 是，保证变量类型一致              | 是，保证变量类型一致                       | 否，可能导致类型*不一致*       |
| *示例*     | `typedef int* int_ptr;` | `enum Color {RED, GREEN, BLUE};` | `#define 1 YES`     |
| *使用场景*   | 定义新的==数据类型==，如指针类型      | 定义一组命名的==整数常量==                  | 定义*常量*或*宏*          |
| *类型检查*   | 编译时进行                   | 编译时进行                            | 无                   |
| *内存分配*   | 可以定义指针类型                | 不分配内存                            | 不分配内存               |
- [[C专家编程#3.2.3 关于枚举|枚举]]
### const
- *修饰常量*：常量的值在**编译**时就已确定，并在运行时保持不变
- *修饰形参*：`func(const int a){}`;该形参在函数里不能改变
- *修饰类成员函数*：`const`类成员函数是不能修改**成员变量**的数值
- *作用域限制*：限制在声明时所在的作用域内部
#### const 不仅仅意味着常数
```c
    const int a;  
    int const a;  
    const int *a;  
    int * const a;  
    int const * const a;
```

前两个的作用是一样，a是一个常整型数。

第三个意味着a是一个指向常整型数的指针（也就是，整型数是不可修改的，但指针可以）。

第四个意思a是一个指向整型数的常指针（也就是说，指针指向的整型数是可以修改的，但*指针是不可修改的*）。

最后一个意味着a是一个指向常整型数的常指针（也就是说，指针指向的整型数是不可修改的，同时指针也是不可修改的）。

#### 与`#define`的区别

- 编译阶段，安全性，内存占用

用`#define max 100`; 定义的常量是没有类型的（不进行**类型安全检查**，可能会产生意想不到的错误），所给出的是一个立即数，编译器只是把所定义的常量值与所定义的常量的名字联系起来，define所定义的宏变量在**预处理**阶段的时候进行替换，在程序中使用到该常量的地方都要进行拷贝替换；

用`const int max = 255 ; `定义的常量有类型（编译时会进行类型检查）名字，存放在内存的**静态区域**中，在编译时确定其值。

在程序运行过程中`const`变量只有==一个拷贝==，而`#define`所定义的宏变量却有==多个拷贝==，所以宏定义在程序运行过程中所消耗的*内存*要比const变量的大得多
### inline
> 在函数**定义**前加上`inline`修饰符可将函数指定为内联函数，且必须在调用之前可见。

- *减少函数调用开销*：以代码膨胀（复制）为代价
- 内联并不一定会展开，由编译器决定。可使用`__attribut__((always_inline))`强制展开
- C++的*模板函数默认内联*：因为需要在编译时具体化
### `#define`
> 使用 `#define` 预处理器指令定义一个宏时，它实际上是将宏名称**替换**为您指定的值。宏的值**默认为 1**。

> 优点
- *开销小*：无参数传递、堆栈操作
- *代码复用*：无函数调用开销
- *条件编译-灵活性*：以`#ifdef` `#ifndef`等结合实现复杂的条件编译逻辑，多平台兼容
- *多参数展开*：轻松处理可变数量的参数
- *跨平台兼容性*：预处理阶段生成特定平台代码
> 缺点
- *预处理*阶段替换，没有类型检查和编译优化，会导致代码膨胀
- 调试时不可见
- 没有作用域限制，全局可见
- 不支持多态、无法实现虚函数

> 运算符

- `#` 运算符会将其后面的参数转换为一个字符串，不包括任何空格和宏的替换结果。
- `##` 是*标记粘贴运算符*，它会去掉它两侧的空格（如果存在的话），并把两侧的标记粘贴在一起。

### extern
- 声明一个在其他文件中定义的外部变量或函数，具有外部链接属性，可以被任何文件访问。
- 编译器在编译时不会为其分配内存，而是在**链接**阶段解析这些引用。
- 允许在当前文件中使用这些*外部变量*或函数而不需要重新定义。
- 和`.h`文件配合避免多个源文件中重复定义
#### 与`.h`的区别
- 用于定义和声明全局变量，函数原型等，以便在多个源文件中使用
- 通过预处理`#define`包含到源文件中
- 每个源文件都有一个局部副本
### extern”C” 的作用

- 在C++中使用C的已编译好的函数模块，这时候就需要用到extern”C”。
- extern在链接阶段起作用（四大阶段：预处理--编译--汇编--链接）。
### static
#### 概念
- 函数内的**静态局部变量**在调用之后保留其值，在函数执行完成之后不会被释放，而是继续保留在内存中。(静态数据区)
- 静态全局变量或函数仅在声明它的文件中“可见”,不用担心与其他文件中的内容冲突
- `static`局部变量分配在**数据段（或称为全局数据区）**，而不是栈区。
#### static的三种位置

- **全局变量中的 `static`**
	只在定义它们的文件内可见，即它们具有文件内限定的生命周期，即使它们在其他文件中被声明为 `extern` 也无法访问。
- **局部变量中的`static`**
	只会在程序*首次*加载到内存时==初始化一次==，并且它在函数调用结束后仍然保留其值，会影响函数的*可重入性*。
- **函数中的`static`**
	限定了函数的作用域，使得函数只能在定义它的文件内被调用。即使函数声明在头文件中，并且该头文件被包含在多个源文件中，全局 `static` 函数也不会造成多定义错误。
### volatile
> 告诉编译器不要对该变量进行优化，每次访问该变量时都从**内存**中读取最新的值。（虽然读写寄存器比读写内存快）

#### 使用volatile
> 使用 `volatile` 关键字可以告诉编译器不要对该变量进行优化，每次访问该变量时都从内存中读取最新的值。这样可以确保在多线程或中断环境下，对该变量的访问是正确的。
- *并行设备*的硬件寄存器（如：状态寄存器）
- 一个*中断*服务子程序中会访问到的非自动变量
- *多线程*应用中被几个任务共享的变量
#### const volatile
- **用途**：它表示该变量的**地址**是不可变的（即不能通过这个指针修改其所指对象的内容），但其值可能在程序的控制之外发生变化。这对于表示**只读**的*状态寄存器*特别有用，这些寄存器的值可以由外部事件（如硬件中断）改变，而程序不应该试图修改它们，但每次访问都需要获取最新的状态。
- **例子**：在嵌入式系统编程中，如果有一个代表传感器读数的寄存器，这个寄存器的内容是由**硬件定期更新**的，但软件不应当去修改它，这时就可以声明为 `const volatile` 类型。
### switch
- `case` 标签必须是整型常量表达式
- 编译时确定其值
- 使用跳转表`jump table`（数组）优化查询，数组索引必须是整数。
### sizeof
`sizeof`是一个**操作符**，也是关键字，*编译*时返回结果，用于获取对象或类型在内存中的固定*字节大小*。

`sizeof`不会对`()`中的数据或者指针做**运算**，检测到对应类型直接返回结果。

`sizeof`无法计算指针字符串的大小，只能计算数组。
```c
char *p = "sadasdasd";
sizeof(p):4//32系统指针大小为4字节
sizeof(*p):1//指向一个char类型

char a[10];
sizeof(a):10  //检测到a是一个数组的类型。
```
### 数组名与指针
#### 数组名
- 在函数内部，数组参数退化为为指向数组首地址的指针。
- 大小固定为整个数组的大小。
- 无法被改变或重新赋值。
- 无法进行指针运算。
- 是一个*常量指针*，大多数情况下，`&array` 和 `&array[0]` 是等价的，指向数据起始位置。
- 在*作为更大结构的一部分（如结构体成员）* 或*指针解引用及运算*时，`&array + 1`会跳过整个数组的大小，而`&array[0] + 1`仅仅会跳过一个元素的大小。
#### 指针
- 是一个*变量*，存储一个内存地址。
- 大小固定为指针类型的大小。
- 可以指向任意类型的对象。
- 可以被改变或重新赋值。
- 可以进行指针运算，如加法、减法等。

## 数据结构

### 数组

对二维数组`a[M][N]`进行遍历时，取决于内存的访问模式，通常先遍历N再遍历M可能更高效。
### 链表
#### 链表和数组的区别
- 链表位于*堆内存*中并动态分配
- 链表依赖于引用，其中每个节点由数据以及对上一个和下一个元素的引用组成
- 数组是存储在顺序内存中的连续内存块
- 数据基于索引，每个元素都与索引相关联
#### 如何优化链表遍历
- *使用辅助数据结构*：在`LRU`缓存实现中，可以使用*散列表*加上链表的方式来减少查找时间，从 $O(n)$ 降到 $O(1)$
### 内存对齐
**平台原因(移植原因)**：不是所有的硬件平台都能访问任意地址上的任意数据的；某些硬件平台只能在某些地址处取某些特定类型的数据，否则抛出硬件异常。

**性能原因**：数据结构(尤其是栈)应该尽可能地在自然边界上对齐。原因在于，为了访问未对齐的内存，处理器需要作两次内存访问；而**对齐**的内存访问仅需要*一次访问*。

#### struct内存对齐
- 对于结构体的各个成员，第一个成员的偏移量是0，排列在后面的成员其当前偏移量必须是**当前成员类型**的*整数倍
- 结构体内所有数据成员各自内存对齐后，结构体本身还要进行一次内存对齐，保证整个结构体占用内存大小是结构体内**最大数据成员**的最小整数倍；

使用64位编译 ，int占4， char 占1， unsigned short 占2，char* 占8，函数指针占8个，由于是64位编译是8字节对齐（说明：按几字节对齐，是根据*结构体的最长类型决定的*，这里是函数指针是最长的字节，所以按8字节对齐）所以该结构体占24个字节。
```c
//64位
struct C 
{ 
 double t;   //8   1111 1111
 char b;  //1      1
 int a;   //4      0001111  
 short c;  //2     11000000
};  
 sizeof(C) = 24;  //注意：1 4 2 不能拼在一起
```

char是1，然后在int之前，地址偏移量*得是4的倍数*，所以char后面补三个字节，也就是char占了4个字节，然后int四个字节，最后是short，只占两个字节，但是总的偏移量得是double的倍数，也就是8的倍数，所以short后面补六个字节

#### union内存对齐
- 联合体是`C`语言中的一种特殊的数据类型，它可以让不同的数据类型**共享**同一个内存空间。您可以定义一个有多个成员的联合体，但是在任何给定的时刻，只有一个成员可以包含**一个值**。
- 找到占用字节最多的成员；
- `union`的字节数必须是占用字节最多的成员的字节的倍数，而且需要能够容纳其他的成员
```c
//x64
typedef union {
    long i;
    int k[5];
    char c;
}D
```

本例中是`long`,占用8个字节,`int k[5]`中都是int类型,仍然是占用4个字节的，然后*union的字节数必须是占用字节最多的成员的字节的倍数*,而且需要能够容纳其他的成员,为了要容纳k(20个字节),就必须要保证是**8**的倍数的同时还要大于20个字节,所以是24个字节。
### 位域

- C语言允许在一个结构体中以位为单位来指定其成员所占内存长度，这种以位为单位的成员称为“位段”或称“位域”( `bit field`) 。利用位段能够用较少的位数存储数据。一个位段必须存储在同一存储单元中，不能跨两个单元。如果第一个单元空间不能容纳下一个位段，则该空间不用，而从下一个单元起存放该位段。
- 位段声明和结构体类似
- 位段的成员必须是`int`、`unsigned int`、`signed int`
- 位段的成员名后边有一个冒号和一个数字

### \_attribute\_((packed)) 取消对齐

GNU C的一大特色就是**attribute**机制。**attribute**可以设置函数属性（Function Attribute）、变量属性（Variable Attribute）和类型属性（Type Attribute）。

**attribute**书写特征是：**attribute**前后都有两个下划线，并且后面会紧跟一对括弧，括弧里面是相应的**attribute**参数。

**跨平台通信时用到。不同平台内存对齐方式不同。如果使用结构体进行平台间的通信，会有问题。**
例如，发送消息的平台上，结构体为`24`字节，接受消息的平台上，此结构体为`32`字节（只是随便举个例子），那么每个变量对应的值就不对了。

不同框架的处理器对齐方式会有不同，这个时候不指定对齐的话，会产生错误结果

```c
typedef struct_data{
   char m:3;
   char n:5;
   short s;
    
   union{
   int a;
   char b;
   };
    
   int h;
}_attribute_((packed)) data_t;
```

`m`和`n`一起，刚好占用一个字节内存，因为后面是short类型变量，所以在short s之前，应该补一个字节。所以`m`和`n`其实是占了两个字节的，然后是short两个个字节，加起来就4个字节，然后联合体占了四个字节，总共8个字节了，最后int h占了四个字节，就是12个字节

## 函数
### 指针函数与函数指针的区别⭐
#### function returning a pointer
> 指针函数是一种返回指针类型的函数。

- 指针函数的返回类型是一个指针，指向特定类型的数据。调用指针函数时，会返回一个指向函数计算结果的指针。
#### pointer to a function
> 函数指针是指向函数的指针变量。
- 函数指针的类型声明中，指针符号 `*` 出现在函数名之前，用于表示函数指针的类型。
- 函数指针可以用于*直接调用*指向的函数，或者作为*参数*传递给其他函数。
```c
#include <stdio.h>
int addIntegers(int a, int b) {
  return a + b;
}
int main() {
  int (*ptr)(int, int);  // 声明一个指向以两个整数为参数并返回整数的函数的指针变量
  ptr = addIntegers;  // 将函数地址赋值给函数指针
  int sum = ptr(5, 3);  // 通过函数指针调用函数
  printf("和: %d\n", sum);
  return 0;
}
输出：和: 8
```

### 库函数
#### strcpy
- 不会检查目标缓冲区的大小，可能会导致缓冲区溢出
```c
/**
 * @brief 复制字符串
 * @param dest 指向目标字符串的指针
 * @param src 指向源字符串的指针
 * @return 指向目标字符串的指针
 * @note 此函数不会检查目标缓冲区的大小，可能会导致缓冲区溢出
 */
char *strcpy(char *dest, const char *src);
```
#### strncpy
- 如果n小于源字符串的长度，则不会在目标字符串末尾添加空字符
```c
/**
 * @brief 复制字符串，最多复制指定数量的字符
 * @param dest 指向目标字符串的指针
 * @param src 指向源字符串的指针
 * @param n 最多复制的字符数
 * @return 指向目标字符串的指针
 * @note 如果n小于源字符串的长度，则不会在目标字符串末尾添加空字符
 */
char *strncpy(char *dest, const char *src, size_t n);
```
#### strncpy_s
- 确保不会超出目标缓冲区的大小，并且会以空字符结尾
```c
/**
 * @brief 安全地复制字符串
 * @param dest 指向目标字符串的指针
 * @param src 指向源字符串的指针
 * @param n 最多复制的字符数
 * @param size 目标缓冲区的大小
 * @return 指向目标字符串的指针
 * @note 此函数会确保不会超出目标缓冲区的大小，并且会以空字符结尾
 */
char *strncpy_s(char *dest, rsize_t size, const char *src, rsize_t n);
```
#### strcat
`strcat` 函数用于将一个字符串附加到另一个字符串的末尾。
```c
/**
 * @brief 将两个字符串连接起来。
 *
 * 该函数将由 @p src 指向的字符串附加到由 @p dest 指向的字符串的末尾。
 * 结果字符串会以空字符终止。
 *
 * @param dest 指向目标数组的指针，用于存储连接后的结果。
 * @param src 指向要附加的源字符串的指针。
 *
 * @return 返回指向 @p dest 中的结果字符串的指针。
 */
char* strcat(char* dest, const char* src);
```
#### strncat
`strncat` 函数用于将一个字符串的前 n 个字符附加到另一个字符串的末尾。
```c
/**
 * @brief 最多从一个字符串中复制 n 个字符到另一个字符串的末尾。
 *
 * 该函数将由 @p src 指向的字符串的前 @p num 个字符附加到由 @p dest 指向的字符串的末尾。
 * 结果字符串会以空字符终止。如果复制的字符少于 @p num 个，因为遇到了空字符，
 * 则不再复制更多的字符。
 *
 * @param dest 指向目标数组的指针，用于存储连接后的结果。
 * @param src 指向要附加的源字符串的指针。
 * @param num 要从 @p src 复制的最大字符数。
 *
 * @return 返回指向 @p dest 中的结果字符串的指针。
 */
char* strncat(char* dest, const char* src, size_t num);
```
#### strstr
`strstr` 函数用于在一个字符串中查找子字符串首次出现的位置。
```c
/**
 * @brief 查找一个字符串中首次出现的子字符串。
 *
 * 该函数返回指向由 @p str 指向的字符串中首次出现的由 @p sub 指向的子字符串的指针。
 * 如果没有找到子字符串，则返回 NULL。
 *
 * @param str 指向要搜索的字符串的指针。
 * @param sub 指向要查找的子字符串的指针。
 *
 * @return 返回指向首次出现的子字符串的指针，如果没有找到则返回 NULL。
 */
char* strstr(const char* str, const char* sub);
```
#### strcmp
- 此函数比较两个字符串，直到遇到第一个不匹配的字符或字符串结束符
```c
/**
 * @brief 比较两个字符串
 * @param str1 指向第一个字符串的指针
 * @param str2 指向第二个字符串的指针
 * @return 比较结果
 * - 返回值小于0：str1小于str2
 * - 返回值等于0：str1等于str2
 * - 返回值大于0：str1大于str2
 * @note 此函数比较两个字符串，直到遇到第一个不匹配的字符或字符串结束符
 */
int strcmp(const char *str1, const char *str2);
```
### memcpy
#### 函数定义
- 不会检查目标缓冲区的大小，可能会导致缓冲区溢出
```c
#include <stddef.h> // For size_t

/**
 * @brief 复制指定数量的字节从一个内存区域到另一个内存区域。
 *
 * 此函数从 @p src 指向的内存区域复制 @p num 字节到 @p dest 指向的内存区域。
 * 如果源和目标区域重叠，此函数的行为与 C 标准库中的 `memcpy` 函数相同。
 *
 * @param dest 指向目标内存区域的指针。
 * @param src 指向源内存区域的指针。
 * @param num 要复制的字节数。
 *
 * @return 返回指向目标内存区域的指针。
 */
void* memcpy(void* dest, const void* src, size_t num) {
    char* d = (char*)dest;
    const char* s = (char*)src;

    if (d <= s && s < (d + num)) { // 源和目标区域有正向重叠
        while (num-- > 0) {
            *d++ = *s++;
        }
    } else if (s < d && d < (s + num)) { // 源和目标区域有反向重叠
        d += num - 1;
        s += num - 1;
        while (num-- > 0) {
            *d-- = *s--;
        }
    } else { // 源和目标区域没有重叠
        while (num-- > 0) {
            *d++ = *s++;
        }
    }

    return dest;
}

```
#### 实现说明
- *正向重叠*：如果源区域开始地址小于等于目标区域开始地址，并且源区域结束地址大于目标区域开始地址，则认为是正向重叠。在这种情况下，我们从左到右逐个字节复制。
- *反向重叠*：如果源区域开始地址小于目标区域开始地址，并且目标区域结束地址大于源区域开始地址，则认为是反向重叠。在这种情况下，我们从右到左逐个字节复制。
- *无重叠*：如果源和目标区域没有重叠，则直接从左到右逐个字节复制。
#### 标准库实现
在实际的C标准库实现中，`memcpy` 可能会使用更高效的算法和技术
- 使用更快速的循环（如使用整数寄存器进行迭代）
- 使用汇编语言指令（如 `rep movsb` 在x86架构上）
- 对齐检测和处理
- 特定平台上的优化技术
- 使用不同的循环来处理小数据块和大数据块，以提高效率。
#### 使用示例
```
/*示例：使用 memcopy 复制字符串*/
memcopy(dest, src, strlen(src) + 1);  /*包括终止符*/
```

### strlen
- `strlen()`是一个`c`库函数，仅对`c`风格字符串有效，直到`'\0'`为止了，计数结果不包括`\0`。

### sprintf

`sprintf`是C语言标准库中的一个函数，用于格式化字符串并将结果输出到字符数组（缓冲区）中，而不是直接输出到标准输出设备（如屏幕）。这个函数在很多方面类似于`printf`，但它允许你将格式化后的字符串保存起来供后续使用。`sprintf`函数的原型如下：

```
int sprintf(char *str, const char *format, ...);
```
- *参数说明*:
    - `str`: 是一个指向字符数组的指针，用于存放格式化后的字符串。这个缓冲区需要足够大以容纳所有格式化后的文本，包括结尾的空字符`\0`。如果缓冲区太小，可能会导致缓冲区溢出，这是安全问题的一个常见来源。
    - `format`: 是一个格式字符串，其中可以包含普通字符和特殊的格式说明符（如`%d`、`%s`、`%f`等），用于指定如何格式化数据。
    - `...`: 是可变参数列表，可以是一个或多个任意类型的参数，它们将根据`format`字符串中的格式说明符进行格式化。

## 编译原理
### `#include<>`和`#include""`的区别
#### `#include<>`
- 用于包含系统提供的*标准库*头文件。
- 在编译器的~~搜索路径~~中寻找头文件。
- #todo 搜索路径
- 编译器会先在系统的标准头文件目录中查找，如果找不到则报错。
#### `#include""`
- 用于包含用户自定义的头文件或项目中使用的其他非系统头文件。
- 在当前源文件的相对路径或指定的绝对路径中寻找头文件。
- 编译器会首先在当前源文件所在目录中查找，如果找不到再根据指定的路径查找。
#### 先引用标准库，然后是自定义头文件
- 从代码的可读性和可维护性的角度，标准库头文件通常是所有程序的*基础*，而自定义头文件则是针对特定的应用程序
#### 头文件的作用
- *源代码保密*
- *加强类型安全检查*：当某个接口实现与声明不一致时，就会提示错误
#### 在头文件中定义静态变量是否可行
> 不可行，易造成资源浪费和重定义错误

- 静态变量不同于声明，同时分配了*存储空间*
- 多个包含该头文件的源文件中都会创建对应的静态变量，导致*重定义*错误
### `#if` `#ifdef`

在C++预处理器中，`#ifdef` 和 `#if` 是两种不同的条件编译指令。

`#ifdef` 检查一个宏是否已经定义，而不管它的值是什么。如果宏已经定义，无论其值是什么，`#ifdef` 条件都会为真。而 `#if` 则会检查宏的值是否为真（非零）。

### `#pragma once`

- **用途**：`#pragma once` 是一个编译器指令，用于告诉编译器在编译过程中只包含一次指定的头文件。这意味着如果头文件被多次包含（直接或间接），编译器会确保它只被包含一次。
- **优点**：`#pragma once` 的语法简单，易于理解和使用。它不需要定义唯一的宏来检查头文件是否已经被包含。
- **缺点**：`#pragma once` 不是C++标准的一部分，而是由各个编译器实现的。这意味着它的行为可能会因编译器而异。虽然大多数现代编译器都支持 `#pragma once`，但在某些特定情况下，它可能不会按预期工作。
### `#error`
> 编译程序时，只要遇到`#error`就会生成一个编译错误提示消息，并停止编译

- `#error error-message`
- 往往有些宏定义是在外部指定的（如`makefile`），或是在系统头文件中指定
```c
#ifdef xxx
...
#error "xxx has been defined"
#else
#endif
```
### `#define`
- 使用 `define`声明个常数，用以表明1年中有多少秒（忽略闰年问题）
```c
#define SECOND_PER_YEAR (60*60*24*365)UL
```
- 定义宏`MIN`,返回输入参数中较小的数
```c
#define MIN(A,B) ((A)<=(B) ? (A) : (B))
```
#### `#`与`##`
#### `#`字符串操作符
- 宏文本替换
```c
#define CONVERT(a) #a

int test_convert=10;
printf(CONVERT(test_convert)); //输出字符串test_convert
```
#### `##`
> 将宏定义的多个形参转换为实际的参数
```c
#define CAT(a,b) a##b

int num5=20;
printf("%d\n",CAT(num,5));//等价于`printf("%d\n",num5);` 输出20
```

### 预处理 -> 编译 -> 汇编 -> 链接 -> 运行
1. **预处理**（Preprocessing）：这是编译过程的第一步，预处理器会根据源代码中的预处理指令（如`#include`、`#define`等）来修改源代码。例如，它会将头文件插入到源代码中，替换宏定义等。经过这一步，源代码会被转换成一个修改过的源代码文件，这个过程并不涉及编译或*语法检查*。
2. **编译**（Compilation）：接下来，修改后的源代码会被编译器处理。编译器将源代码（通常是高级语言，如C或C++）转换为汇编代码，这是一个与特定计算机架构相关的低级、人类可读的语言。
3. **汇编**（Assembly）：随后，汇编器将汇编代码转换为目标代码（机器代码），这是计算机可以直接执行的二进制格式，但通常还是分散的代码段和数据段。
4. **链接**（Linking）：最后，链接器将一个或多个目标文件以及任何必要的库文件合并起来，解决符号引用，形成一个完整的可执行文件。这包括分配地址空间、解析未定义的符号（如外部函数或变量）等。经过链接后，就得到了最终的可执行文件（如`.exe`文件在Windows系统中，或没有扩展名的文件在Unix-like系统中）。
5. **运行**（Execution）：至此，生成的可执行文件可以在相应的操作系统环境下被加载并执行。

### 使用32位编译情况下，给出判断所使用机器大小端的方法

> 在大端模式（`Big-Endian`）中，多字节数据的==最高字节==（*最左边的字节*）存储在==最低内存地址==上
> 而最低字节（最右边的字节）存储在最高的内存地址上。

- 联合体方法判断方法：利用`union`结构体的从低地址开始存，且同一时间内只有一个成员占有内存的特性。*大端储存符合阅读习惯*。联合体占用内存是最大的那个，和结构体不一样。

a和c公用同一片内存区域，所以更改c，必然会影响a的数据
```c
#include<stdio.h>

int main(){
  union w
  {
      int a;
      char b;
  }c;
  c.a = 1;
  if(c.b == 1)
   printf("小端存储\n");
  else
   printf("大端存储\n");
 return 0;
}
```

- 指针方法
通过将`int`强制类型转换成`char`单字节，p指向a的起始字节（低字节）
```c
#include <stdio.h>
int main ()
{
    int a = 1;
    char *p = (char *)&a;
    if(*p == 1)
    {
        printf("小端存储\n");
    }
    else
    {
        printf("大端存储\n");
    }
    return 0;
}
```

### 用变量a给定下面的定义
```c
a) 一个整型数；
b）一个指向整型数的指针；
c）一个指向指针的指针，它指向的指针是指向一个整型数；
d）一个有10个整型的数组；
e）一个有10个指针的数组，该指针是指向一个整型数；
f）一个指向有10个整型数数组的指针；
g）一个指向函数的指针，该函数有一个整型参数并返回一个整型数；
h）一个有10个指针的数组，该指针指向一个函数，该函数有一个整型参数并返回一个整型数
答案：
a)int a
b)int *a;
c)int **a;
d)int a[10];
e)int *a [10];
f) int a[10], *p=a;
g)int (*a)(int)
h) int( *a[10])(int)
```
### 可执行文件组成
> 一个程序本质上都是由 bss段、data段、text段三个段组成

#### .bss段
> `bss`段（`Block Started by Symbol segment`）通常是指用来存放程序中**未初始化**的全局变量的一块内存区域。

- 一般在初始化时`bss `段部分将会清零（bss段属于**静态内存分配**，即程序一开始就将其清零了）
- bss段不在可执行文件中，由系统初始化。
#### .data段
> 存放在**编译**阶段(而非运行时)就能确定的数据，可读可写。

也是通常所说的静态存储区，赋了初值的全局变量、常量和静态变量都存放在这个域
#### .text段
> 存放程序代码的区域， 编译时确定， 只读

- 存放处理器的**机器指令**，当各个源文件单独编译之后生成目标文件，经连接器链接各个目标文件并解决各个源文件之间函数的引用
- 当我们的可执行程序被加载到内存以后，通常都会将`.text`段所在的内存空间设置为*只读*，以保护`.text`中的代码不会被意外的改写（比如在程序出错时）
### 与或非，异或。运算符优先级

```c
sum=a&b<<c+a^c;
```

其中a=3,b=5,c=4（先加再移位再`&`再异或）答案4

## 其他
### 有符号和无符号运算
> 有符号和无符号运算，强制转换为`无符号 `
### 短路
if语句中如果是或运算（` | `），第一个条件满足时，第二个条件还会判断吗。或运算的话，当然不会，因为 `0|1=1`，中断了。
### 原码 反码 补码
- *原码*：最高位表示符号，其余位表示数值的绝对值。正数的原码就是二进制表示，符号位为0。负数的原码符号位为1，数值位根据绝对值的二进制表示。
- *反码*：正数的反码与原码相同，负数的反码是对原码除符号位外的每一位取反（0 变为 1，1 变为 0）。
- *补码*：正数的补码与原码和反码相同，负数的补码是对反码加 1。