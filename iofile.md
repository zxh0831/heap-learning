# IO_FILE_Eploitation

## FILE 结构

### FILE 介绍
形成以_IO_list_all为表头的链表，初始化有stdin（标准输入），stdout（标准输出），stderr（标准错误输出流）三个
### fread

### fwrite

### fopen
1.使用 malloc 分配 FILE 结构
2.设置 FILE 结构的 vtable
3.初始化分配的 FILE 结构
4.将初始化的 FILE 结构链入 FILE 结构链表中
5.调用系统调用打开文件

### fclose
与fopen过程类似
### printf/puts
本质是fwrite

## [伪造vtable劫持程序流程](https://ctf-wiki.org/pwn/linux/user-mode/io-file/fake-vtable-exploit/)

### 简介

修改vtable
因此 vtable 劫持分为两种，一种是直接改写 vtable 中的函数指针，通过**任意地址**写就可以实现。
另一种是**覆盖** vtable 的指针指向我们控制的内存，然后在其中布置函数指针。
### 实践

1.往vatble表中写入system
2.往vatble写入fake_vatble
第一个参数传入到位置是——IO_FILE_plus的地址
利用fwrite直接在该位置写参数"sh"
### 2018 HCTF the_end
1.什么是缓冲区
这篇[blog](https://blog.csdn.net/fuhanghang/article/details/109756207)
内存空间的一部分，用来缓存输入或输出的数据,加快速度的读取

2.fflush函数

刷新缓冲区
刷新的方法：

1、缓冲区满时；
2、执行flush语句；（flush：冲洗，冲走）
3、执行endl语句；（end line
4、关闭文件。


3._setbuf函数

4.exit（1377）

返回1377到主线程

5.close函数

0,1,2分别表示stdin，stdout，stderr

6.read函数和write函数   

读取到回车为止（read）

#### 基本信息

#### 思路
先获得libc基地址，通过gdb可以算出各部分相对libc的偏移地址
先得到stdout下vtable的地址
向指向vtable的指针写入fake_vtable 的地址
得到setbuf的地址
往setbuf地址写入；one_gadget地址   


## fsop
思想劫持_IO_list_all的值来伪造链表和其中的_IO_FILE,伪造了数据还需要触发。
触发通过调用_IO_flush_all_lockp,这个函数会触发了链表中所有项的文件流。相当于调用每个函数的fflush（_IO_overflow）
以下几种情况触发：
1. 当 libc 执行 abort 流程时

2. 当执行 exit 函数时

3. 当执行流从 main 函数返回时

### 实例


