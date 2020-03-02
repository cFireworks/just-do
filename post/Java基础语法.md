### Java 8的新特性？
- `Lambda 表达式` − Lambda 允许把函数作为一个方法的参数（函数作为参数传递到方法中）。

- `方法引用` − 方法引用提供了非常有用的语法，可以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合使用，方法引用可以使语言的构造更紧凑简洁，减少冗余代码。

- `默认方法` − 默认方法就是一个在接口里面有了一个实现的方法。

- `新工具` − 新的编译工具，如：Nashorn引擎 jjs、 类依赖分析器jdeps。

- `Stream API` −新添加的Stream API（java.util.stream） 把真正的函数式编程风格引入到Java中。

- `Date Time API` − 加强对日期与时间的处理。

- `Optional 类` − Optional 类已经成为 Java 8 类库的一部分，用来解决空指针异常。

- `Nashorn, JavaScript 引擎` − Java 8提供了一个新的Nashorn javascript引擎，它允许我们在JVM上运行特定的javascript应用。

### <a name="Title.01">一、Java的基本程序设计结构</a>
### 九种基本数据类型的大小，以及他们的封装类?
| 数据类型 | 封装类 | 大小 | 
| :---------: | :---: | :---:| 
| byte  | Byte | 1个字节 |
| boolean  | Boolean  | 1个bit |
| char  | Charactor | 2个字节 |
| int  | Integer | 4个字节 |
| short | Short | 2个字节 |
| long | Long | 8个字节 |
| float | Float | 4个字节 |
| double | Double | 8个字节 |
| void | - | - |


### <a name="Title.02">二、对象与类</a>
### Java面向对象的三个特征与含义？
- `继承`：**继承是从已有类得到继承信息创建新类的过程。**提供继承信息的类被称为父类（超类、基类）；得到继承信息的类被称为子类（派生类）。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段。继承中最常使用的两个关键字是extends（用于基本类和抽象类）和implements（用于接口）。**Java中类的继承是单一继承，若使用extends只允许有一个父类，使用implements则不限。**

- `封装`：**通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。**面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口。

> 四种访问控制符：
>1. 默认的，也称为`default`，在同一包内可见，不使用任何修饰符。
>2. 私有的，以`private`修饰符指定，在同一类内可见。
>3. 公有的，以`public`修饰符指定，对所有类可见。
>4. 受保护的，以`protected`修饰符指定，对同一包内的类和所有子类可见。

- `多态`：**多态性是指允许不同子类型的对象对同一消息作出不同的响应。**简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。**方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。**




### <a name="Title.03">三、继承</a>
### <a name="Title.03.1">3.1 Java Object类的公有函数</a>
### <a name="JO.01">toString()</a>
用于显示调用输出对象信息，或者`this + "string"`字符串重载`+`运算符形式，将`this`转为`String`类型（隐式调用）。  
  
### <a name="JO.02">hashCode()</a>
用于`HashMap`中元素增删改查时`Key`的`Hash`操作。
> JDK`HashMap`的`hash()`源码如下
  
```java
/**
 * Retrieve object hash code and applies a supplemental hash function to the
 * result hash, which defends against poor quality hash functions.  This is
 * critical because HashMap uses power-of-two length hash tables, that
 * otherwise encounter collisions for hashCodes that do not differ
 * in lower bits. Note: Null keys always map to hash 0, thus index 0.
 */
final int hash(Object k) {
    int h = 0;
    if (useAltHashing) {
        if (k instanceof String) {
            return sun.misc.Hashing.stringHash32((String) k);
        }
        h = hashSeed;
    }

    h ^= k.hashCode();

    // This function ensures that hashCodes that differ only by
    // constant multiples at each bit position have a bounded
    // number of collisions (approximately 8 at default load factor).
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);
}
```
  
重写`hashCode()`函数是一个考点，需要注意一些细节。  
> 重写`hashCode()`函数
  
```java
public int hashCode() {
	return id != null ? id.hashCode() : 0;
    // 或者自定义Hash算法
}
```
  
### <a name="JO.03">equals()</a>
用于对象相等测试，比如容器`indexOf()`、`remove()`、`contains()`等函数中。  
> JDK`ArrayList`的`indexOf()`源码如下
  
```java
/**
 * Returns the index of the first occurrence of the specified element
 * in this list, or -1 if this list does not contain the element.
 * More formally, returns the lowest index <tt>i</tt> such that
 * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>,
 * or -1 if there is no such index.
 */
public int indexOf(Object o) {
    if (o == null) {
        for (int i = 0; i < size; i++)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = 0; i < size; i++)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
```
  
重写`equals()`函数是一个考点，需要注意一些细节。  
> 重写`equals()`函数
  
```java
public boolean equals(Object o) {
	// 判断自己比较自己
    if (this == o) {
    	return true;
    }
    // 判断参数，判断参数Class对象与自己Class对象
    if (o == null || getClass() != o.getClass()) {
    	return false;
    }
    A a = (A) o;
    // 判断待比较字段
    if (id != null) {
    	return id.equals(a.id);
    } else {
    	return a.id == null;
    }
}
```
  
### <a name="JO.04">clone()</a>
注意浅复制与深复制。  
  
Object中默认的实现是一个浅复制，如果要实现深复制，必须对类中可变域生成新的实例。  
  
重写`clone()`，同时还应该实现标志接口`Cloneable`，当对象存在组合关系时，需要考虑组合对象的`Clone`。  
> 示例（其实`Clone()`用的不多）
  
```java
class ClassA implements Cloneable {
    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
...
class ClassB implements Cloneable {
    ClassA a;

    @Override
    public Object clone() throws CloneNotSupportedException {
        ClassB b = (ClassB) super.clone();
        if (a != null) {
            b.a = (ClassA) a.clone();
        }
        return b;
    }
}
```
  
### <a name="JO.05">wait()</a>
用于多线程同步，阻塞线程，注意：`wait()`函数的调用必须先获取`锁`。  
  
### <a name="JO.06">notify()/notifyAll()</a>
用于多线程同步，唤醒线程，注意：`notify()`/`notifyAll()`函数的调用必须先获取`锁`（与`wait()`调用时同一个`锁`）。  
> 典型用法
  
```java
// 线程一
synchronized(shareMonitor) {
	while (conditionIsNotMet) {
    	shareMonitor.wait();
    }
}
...
// 线程二
synchronized(shareMonitor) {
    shareMonitor.notifyAll();
}
```
  
> 当然可以使用显示的`Lock`、`Condition`对象
  
```java
Lock lock = new ReentrantLock();
Condition cond = lock.newCondition();
...
// 线程一
lock.lock();
try {
	while (conditionIsNotMet) {
    	cond.await();
    }
} finally {
	lock.unlock();
}
...
// 线程二
lock.lock();
try {
	cond.signal();
} finally {
	lock.unlock();
}
```
  
### <a name="JO.07">getClass()</a>
获取`Class`对象，它包含了与类有关的信息，用于`RTTI`。事实上，Class对象就是用来创建类的所有`常规`对象的。  
  
每一个类都有一个Class对象。  
  
### <a name="JO.08">fianlize()</a>
一旦垃圾回收器准备好释放对象占用的存储空间，将首先调用其`finalize`方法，并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存。  
  
潜在的编程陷进：将`finalize()`等同于C++析构函数。对象被回收的时机是不确定的，也可能永远不会被回收，如果资源的释放依赖于`finalize()`，那么释放可能永远也不会发生。 

### <a name="Title.04">四、接口、lambda表达式与内部类</a>