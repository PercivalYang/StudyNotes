# Chapter 1 (code: intro)
- 第一章主要介绍整本书的架构，以及每个模块的学习目的
## Virtualization memory 
- 物理地址(physical memory)是共享的资源，每一个进程访问其自己的virtual address space(或者称address space)，不会干涉到其他进程的address space；
- 那么是如何实现每个进程拥有独立的address space?
## Concurrency
### Thread
> You can think of a thread as a function running within the same memory space as other functions, with more than one of them active at a time.
- 理解：在同一个内存空间中，同时执行多个function(thread)
- Q：OS如何让程序实现高并发，同时每个process运行时不会干扰到其他process
# Persistance
- Q：OS如何实现File System的，包含I/O设备的访问，user mode、root mode、system call等的细节是什么？

# Chapter 2
## Terms 
- mechanism: 在low-level层面执行的methods或者protocols，例如context switch的time-sharing
- policies: 是在mechanism之上，在OS内通过算法进行一系列决定，例如给定一堆programs，OS应该运行哪些program？
## Process APIs
- Create
- Destroy
- Wait
- Miscellaneous Control：例如suspend和resume(continue process running)
- Status
### Creation 
- 第一步: Programs都是以executable format存储在disk中，OS需要通过load process将instructions和static data(e.g. Initialized variables)加载到memory中；
    - loading process的两种形式：
        - eagerly：一次性将所有instructions和static data加载到memory；
        - lazily: 只加载Program execution 所需要的instructions和data
- 第二步: allocate **run-time stack**，C语言中用来存储local
variables, function parameters, and return addresses
- 第三步: 初始化一些memory用来作为Program's heap, C 语言中的heap通过`malloc()`和`free()`来动态的收集、释放data；
- 第四步：同样还会对I/O进行初始化，例如每个Process默认的三个file descriptors: stdin, stdout, error。
- 最后：上述完成后，执行Program需要指定一个**entry-point**，通常是`main()`，然后通过**mechanism**跳转到entry-point**后，OS会将CPU的权限转交给这个new-created process，进而执行program
- 