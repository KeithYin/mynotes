# 寄存器



# 应用程序眼中的操作系统

* 什么是应用程序







# 硬件眼中的操作系统

> 状态及、C程序

>  https://software.intel.com/content/www/us/en/develop/articles/intel-sdm.html#nine-volume
>
> intel处理器 cpu reset 之后的状态可以从手册中读到。最需要关注的是 3a
>
> https://software.intel.com/content/www/us/en/develop/download/intel-64-and-ia-32-architectures-sdm-volume-3a-system-programming-guide-part-1.html

为了让计算机可以执行我们的任意程序，一定会存在一些软件/硬件约定

* CPU reset 后，处理器出于某个确定的状态。`9.1.1 Processor State After Reset`
  * PC指针一般指向一段 memory-mapped ROM
    * ROM存储了厂商提供的firm-ware
  * 处理器的大部分功能处于关闭状态
    * 缓存，虚拟存储，。。。
* Firmware：U-boot(开源项目)
  * 将用户数据加载到内存（磁盘上的数据->内存）
    * 例如存储介质上的第二级loader
    * 活着直接加载操作系统（嵌入式）
  * Legcy bios 约定
    * legacy bios 会将引导盘第一个扇区（主引导扇区，MBR）加载到内存 `7c00` 位置
      * 处理器处于 16-bit 模式
      * 然后将 CS:IP 指向 `7c00`. 之后 cpu 就会执行 主引导扇区的代码了。
    * 其它没有任何约束
    * https://www.usenix.org/legacy/event/usenix05/tech/freenix/full_papers/bellard/bellard.pdf
    * Seabios: qemu 默认的bios



关于系统镜像文件：

* 前512字节，特殊的主引导扇区 （一段汇编代码，用来设置cpu。然后执行加载kernel到内存，这段代码是用c写的。）
  * cpu 16-bit 到 32-bit 的切换
  * Elf32 & elf64的加载器
    * 约定：main函数参数位于磁盘的 512字节～
    * ELF位于磁盘的 512 + 1024 字节～
* 然后 1024 字节的空白：main函数的参数
* 然后是 elf 文件



关于主引导扇区：一般做什么操作呢？

* 



# 操作系统中的互斥

> 注意不是应用程序的互斥哦。



# malloc & free

* 进程会向操作系统请求一个大块内存

* 请求到之后

* 进程会从这个大块内存中进行分配。（再分配的代码由用户编写）

* 操作系统提供两个系统调用

  * ```c++
    void *sbrk(intptr_t increment); //古代方法 brk
    void *mmap(void *addr, size_t length, int port, int flags, int fd, off_t offset); //现代方法
    ```



如果要实现 malloc & free 应该怎么做？

* 首先理解问题是什么，然后再谈解决方案。（使用好 trace/profiler 确定问题是什么。



# 同步

* 自旋锁
* 条件变量
* 信号量（比条件变量high level）
* 



# 调试理论给我们的启示

> 什么是bug：程序执行的结果与期望的结果不符
>
> 如何找到bug：将bug 转成 程序错误（assertions）等等。。。



**程序运行本质是状态机**



找到bug的两个关键链条

* Fault(bug) -> Error (程序状态错误)。
  * 即：如何将bug转成程序错误
  * 需要更多的测试
    * 构造复杂的 workloads
* Error(程序状态错) -> Failure(可观测的结果错)
  * 需要更多的检查
    * **随时开关各类日志信息**
    * 各种防御性的 assertions



# 并发BUGS

> 死锁：
>
> 原子性违反：忘记上锁🔒
>
> 顺序违反：忘记同步

死锁的类型

* AA 死锁：同一个线程获取同一个锁两次
* ABBA死锁：一个线程先获取 A 再获取 B，另一个线程先获取 B 再获取 A





# 操作系统

> 管理程序运行的软件
>
> 应用程序：代码+数据（文件）=状态机
>
> *  状态
>   * 内存 + 寄存器
>
> 其实应用程序自己能干的事情有限：只能读取内存的值，然后计算。
>
> 其余的功能都是由操作系统提供的：内存申请，读文件，写文件，。。。
>
> 应用程序眼中的操作系统：仅仅是一堆系统调用API。
>
> 硬件眼中的操作系统：一段C程序代码。

操作系统相比应用程序有哪些特殊：

* 可以看到整块物理内存；应用程序看到的是虚拟内存
* 可以使用一些系统寄存器（`CR0, CR3, ...`）
* 可以关掉系统中断，应用程序是不可以关系统中断的。
* 将应用程序的内存 和 寄存器现场加载到CPU上执行。（实际上一个iret指令好像就可以。



操作系统如何调度多个程序执行

* 将应用程序代码加载到内存，
* 然后将 EIP 寄存器指向对应的 内存地址。这样CPU就执行应用程序的代码了
* 时钟⏰中断来了，CPU又开始执行操作系统指令了，
  * 这时操作系统会保存上一个程序的上下文。
  * 然后调度执行下一个应用程序。
* 。。。



# 进程抽象

> 程序 = 状态机
>
> 进程 = 状态机的执行
>
> 操作系统 = 状态机的模拟器 + 系统调用
>
> Fork: 将进程看作状态机后的 一次 状态拷贝：代码、数据、栈、寄存器。
>
> fork：非常浪费资源。
>
> execve
>
> exit

exit：

* `exit(0)`: `stdlib.h` 中声明的 `libc` 函数
  * 会调用 `atexit`
* `_exit(0)`: `glibc` 的系统调用 `wrapper`
  * 会调用 `exit_group` 终止整个进程
  * 不会调用 `atexit`
* `syscall(SYS_exit, 0)`
  * 执行 `exit` 系统调用，终止当前线程
  * 不会调用 `atexit`
* 很复杂：区分好 库函数（应用程序一部分）和系统调用即可。



# 操作系统中的进程

> 进程执行指令需要 代码、数据、堆栈
>
> * 代码
> * 数据：字符串常量
> * 堆栈：函数调用

```c
/*
main 在代码区
x 在数据区
y 在堆栈上
*/
int main(int argc, char* argv[]) {
  static int x = 10;
  int y = 100;
}
```



**`进程的地址空间` **

* 代码、数据、堆栈（都可以通过指针访问
* 动态链接库
* 运行时分配内存
* 进程的地址空间可以理解成为一段一段连续的内存，每段内存都有自己的职责（代码，数据，堆栈，动态链接库，动态分配的内存，etc。
  * 每段内存有自己的权限 & 大小

> 程序在运行的时候是可以动态加载动态链接库的，就是将一段未知的代码加载到内存中。操作系统如何进行进程的地址空间的创建和修改呢？



# 虚拟化

> 分页机制：硬件提供
>
> 操作系统利用分页机制实现了虚拟地址空间



> 链接与加载
>
> * 需求：允许引用其它文件(C标准称之为编译单元，compilation unit) 里定义的符号
>   * C并不阻止你随便声明符号的类型
>   * 但是类型不匹配会是 undefined behavior

```c
// a.c
int foo(int a, int b) {
  return a + b;
}
```

```c
// b.c
int x = 100, y = 200;
```



```c
// main.c
extern int x, y;
int foo(int a, int b);
int main(int argv, char* argc[]) {
  foo(x, y);
}
```



静态ELF加载器：加载 `a.out` 的执行

* 类似于 Abstract Machine 中的 boot loader
  * 根据ELF program header，将文件中指定的部分移动到内存
  * 操作系统在执行 `execve` 的时候做了啥？
    * 操作系统在内核态调用 `mmap` 
      * 进程还没有准备好，由内核直接执行系统调用
      * 映射好 `a.out` 代码、数据、堆区、堆栈、vvar、vdso、vsyscall
    * 更简单的实现：直接读入进程地址空间



# 常用工具

* gdb notes: http://csapp.cs.cmu.edu/3e/docs/gdbnotes-x86-64.pdf
* `nm` : 查看符号
* `strings`：查看字符串常量
* `size`：查看文件大小
* `strace`：追踪系统调用
* `ltrace`:  追踪库函数调用
* `objdump`：查看二进制文件
* `readelf`：查看elf
* `make`
  * `-nB` 不实际进行编译，只是将编译过程打印出来
* `pmap $pid`: 查看内存的映射。

