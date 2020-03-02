## Java并发编程

### 目录
* [线程管理](./Java并发编程.md#JC.01)
* [线程同步基础](./Java并发编程.md#JC.02)
* [线程同步工具](./Java并发编程.md#JC.03)
* [线程执行器](./Java并发编程.md#JC.04)


### <a name="JC.01">一、线程管理</a>
### 1.0 线程与进程
进程是执行期的程序以及相关资源的总称，通常包含执行代码、打开的文件、挂起的信号、内核内部数据、处理器状态、一个或多个具有内存映射的地址空间以及一个或多个执行线程，还有存放全局变量的数据段。

**线程是在进程中活动的对象，每个线程都拥有一个独立的程序计数器、进程栈和一组寄存器。**内核调度的对象是线程，而不是进程。

在Linux中，线程是一种特殊的进程，它与其他进程共享某些资源：
```c
// Linux通过clone()系统调用实现复制父进程以创建新进程
// 线程的创建，在调用clone()时需传递一些参数标志来指明共享资源
clone(CLONE_VM | CLONE_FS | CLONE_FILES | CLONE_SIGHAND, 0)
```
### 1.1 线程创建
1. 直接继承`Thread`类，重写`run()`方法。
2. 构建一个实现`Runable()`接口的类，重写`run()`方法，以该类创建的实例作为构造参数创建`Thread`类对象。
### 1.2 控制线程中断
1. 线程中断：Java中的一种机制来向线程表明想要终止它，线程对象检查中断请求并决定是否响应中断请求。
2. 中断检测：Java提供`InterruptedException`异常，线程在执行中断`task.interrupt()`后将抛出该异常，`run()`方法可以捕获该异常。
### 1.3 线程的状态
1. 休眠：`thread.sleep()`或者`TimeUnit.SECONDS.sleep()`
2. 等待线程结束：`thread.join()`,发起线程等待thread线程执行结束。
3. 守护线程：`thread.setDaemon()`，只能在`start()`方法前调用，一旦线程开始，daemon状态不可改变。此时调用`setDaemon()`，将抛出`IllegalThreadStateException`异常。

### <a name="JC.02">二、线程同步基础</a>
### 2.1 方法同步
1. **竞态条件**：多个线程同时访问一个共享资源时，竞态条件随之产生。
2. **临界区**：一个用于访问共享资源的代码块，并且同一时刻只能由一个线程执行。
3. **两种基本同步机制**：*关键词synchronized* 和 *Lock接口及其实现类*

### 2.2 关键词synchronized
1. **修饰一个方法：** 此时synchronized绑定的对象是对象实例本身，隐式绑定。
2. **修饰静态方法：** 此时synchronized绑定的对象是该类对象，而非类对象的实例。
3. **修饰代码块：** 绑定一个普通对象实例。
4. **同步代码块中使用条件：** 基于Object类上提供的wait()让线程进入休眠状态，notify()和notifyAll()唤醒休眠状态的线程。

### 2.3 锁机制
`java.util.concurrent.locks`中定义的`Lock机制`比`synchronized机制`更加强大和灵活。synchronized仅能作用在一段结构化代码中，Lock接口可以适应更复杂的代码结构。具有`tryLock()`方法、`ReadWriteLock接口`等，提供新特性。性能更优越。
1. `ReentrantLock类`：
2. `ReadWriteLock接口`及其实现`类ReentrantReadWriteLock`：
3. `StampedLock类`:
4. `Semaphore`：是一个计数器，用来控制一个或多个共享资源的访问。
5. `CountDownLatch`：可以使一个线程等待多个操作结束。
6. `CyclicBarrier`：可以使多个线程在一个共同状态点同步。
7. `Phaser`：可以分阶段地控制并发任务的执行。只有所有的线程都完成一个阶段后，才能继续下一个阶段。
8. `Exchanger`：能够使得两个线程在某个点进行数据交换。
9. `CompletableFuture`：一个或多个任务等待另外一些任务执行结束，并且这些任务将以异步方式运行。

### 2.4 比较Synchronized，ReentrantLock
### 背景，应该就是`Synchronized`的缺点
* `Synchronized`产生原因，`原子性(Atomicity)`与`可见性(visibility)`，其中可见性涉及到`JMM`的`happens-before`原语，这又涉及到`Memory Barrier`，推荐这篇文章[《并发导论》][1]

* `Synchronized`使用示例

	```java
    synchronized (lockObject) {
		// update object state
	}
	```

* 但是`Synchronized`存在的缺点是：它无法中断一个正在等候获得锁的线程，也无法通过投票得到锁...同时多个线程争用同一个锁，jvm的总体开销有点大

### `ReentrantLock`
* 默认是不公平(`unfair`)锁
* 有一个与锁相关的`获取计数器`，获取一次加一，获取两次加二，注意释放两次才代表真正释放锁

* `ReentrantLock`使用示例

	```java
    ReentrantLock lock = new ReentrantLock();
    lock.lock();
    try {
    	// update object state
    }
    finally {
    	lock.unlock();
    }
	```

* 相比较`Synchronized`，`ReentrantLock`在调度的开支上花的时间相对少，从而为更高的吞吐率留下空间，实现了更有效的CPU利用

### `Condition`
* 对比`JDK 1.4`以前版本中的`Object.wait()`，`Object.notify()`，`Object.notifyAll()`，上述三种线程间同步方式有问题，可问题是什么，需要继续学习，留在`todolist`...

* `Condition`提供`await()`，`signal()`，`signalAll()`用于实现上述功能

* `Condition`使用示例，需要和`ReentrantLock`配合，简单使用参见[《关于JAVA Condition 条件变量》][3]
	```java
    ReentrantLock lock = new ReentrantLock();
    Condition condition = lock.newCondition();
    lock.lock();
    try {
    	condition.await();
    	// update object state
    }
    finally {
    	lock.unlock();
    }
    
    condition.signal();
    condition.signalAll();
    ```
### 那么在什么时候使用`ReentrantLock`
* 需要实现`时间锁等候`，`可中断锁等候`，`无块结构锁`，`多个条件变量`，`锁投票`，`高度争用`的情形