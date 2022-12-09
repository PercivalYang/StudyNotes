[toc]
## Thread
### Virtual Thread
- 当创建一个`Thread`类型的变量时，实际上是创建了一个由JVM管理的**Virtual Thread**。即使电脑CPU物理上只支持4个线程，Virtual Threads 可以让程序创建10,000个`Threads`
### Thread API
- `Thread(Runnable fun)`: 创建一个`Thread`时，传入其中的应是`Runnable`，可以是具体的implements了`Runnable`的class，或者Lambda Expression;
- `.start()`: 让该Thread开始运行；
- `.join()`：等待该Thread运行完后，将该线程的运行结果合并(join)到当前“环境”
### Thread Pools
#### Benefits
- 控制系统调用的线程个数
- 重新调用work done的thread，节省重新创建新Thread的时间与内存
#### Create Thread Pools
- 使用[`Executors`](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/util/concurrent/Executors.html) API，例如：  
    - 创建可以reuse thread，但没有限制thread个数的pool：
    ```java
    ExecutorService pool = Executors.newCachedThreadPool();
    ```
    - 创建限制个数为12的pools:
    ```java
    ExecutorService pool = Executors.newFixedThreadPool(12);
    ```
#### How to use pool
- 

## Feature
- Java 利用Feature类型的变量来表示异步计算的结果(result of asynchronous computation)

## CountDownLatch
- `CountDownLatch`是用来确保在task运行前需等待其他Thread运行完才能开始；
- 当创建了`CountDownLatch`的对象时，可以指定需要等待的threads，这些threads在处于完成或准备状态时会调用`CountDownLatch.coutDonw()`来进行“倒计时”，一旦倒计时到达0这些等待的threads就会开始运行
