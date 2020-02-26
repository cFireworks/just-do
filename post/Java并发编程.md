## Java并发编程

### 目录
* [线程管理](./Java并发编程.md#JC.01)
* [线程同步基础](./Java并发编程.md#JC.02)
* [线程同步工具](./Java并发编程.md#JC.03)
* [线程执行器](./Java并发编程.md#JC.04)
* [HashSet](./Java并发编程.md#JC.05)
* [TreeSet](./Java并发编程.md#JC.06)
* [LinkedHashSet](./Java并发编程.md#JC.07)
* [HashMap](./Java并发编程.md#JC.08)
* [HashTable](./Java并发编程.md#JC.09)
* [TreeMap](./Java并发编程.md#JC.10)
* [LinkedHashMap](./Java并发编程.md#JC.11)
* [非线程安全容器类小节](./Java并发编程.md#JC.12)
* [ConcurrentHashMap](./Java并发编程.md#JC.13)
* [CopyOnWriteArrayList](./Java并发编程.md#JC.14)
* [CopyOnWriteArraySet](./Java并发编程.md#JC.15)
* [ArrayBlockingQueue](./Java并发编程.md#JC.16)
* [LinkedBlockingQueue](./Java并发编程.md#JC.17)

### <a name="JC.01">1.线程管理</a>
#### 1.0 线程与进程
进程是执行期的程序以及相关资源的总称，通常包含执行代码、打开的文件、挂起的信号、内核内部数据、处理器状态、一个或多个具有内存映射的地址空间以及一个或多个执行线程，还有存放全局变量的数据段。

**线程是在进程中活动的对象，每个线程都拥有一个独立的程序计数器、进程栈和一组寄存器。**内核调度的对象是线程，而不是进程。

在Linux中，线程是一种特殊的进程，它与其他进程共享某些资源：
```c
// Linux通过clone()系统调用实现复制父进程以创建新进程
// 线程的创建，在调用clone()时需传递一些参数标志来指明共享资源
clone(CLONE_VM | CLONE_FS | CLONE_FILES | CLONE_SIGHAND, 0)
```
#### 1.1 线程创建
1. 直接继承`Thread`类，重写`run()`方法。
2. 构建一个实现`Runable()`接口的类，重写`run()`方法，以该类创建的实例作为构造参数创建`Thread`类对象。
#### 1.2 控制线程中断
1. 线程中断：Java中的一种机制来向线程表明想要终止它，线程对象检查中断请求并决定是否响应中断请求。
2. 中断检测：Java提供`InterruptedException`异常，线程在执行中断`task.interrupt()`后将抛出该异常，`run()`方法可以捕获该异常。
#### 1.3 线程的状态
1. 休眠：`thread.sleep()`或者`TimeUnit.SECONDS.sleep()`
2. 等待线程结束：`thread.join()`,发起线程等待thread线程执行结束。
3. 守护线程：`thread.setDaemon()`，只能在start()方法前调用，一旦线程开始，daemon状态不可改变。此时调用`setDaemon()`，将抛出`IllegalThreadStateException`异常。
