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

### static关键词
- 如果将一个字段定义为static，那么每个类只有一个这样的字段，而对于非静态的实例字段，每个对象有一个自己的副本。
- 静态常量
- 静态方法，static关键字修饰的方法，静态方法不是在对象上执行的方法，可以认为静态方法没有this参数，静态方法只能访问类的静态字段。
> - 静态方法不能重写，静态方法的调用在字节码中是使用INVOKESTATIC，此方法则是直接调用方法区中静态方法，无需经过方法表，因此静态方法的执行只看静态类型，而与实际类型无关，又因为重写的方法调用看的是实际类型，所以静态方法不能被重写。

### StringBuffer和StringBuilder的区别
> - StringBuffer和StringBuilder都可以用来构建字符序列。
> - StringBuffer是线程安全的，默认初始容量是16个字符，通过sychronized修饰的方法使得其方法都是线程安全的。
> - StringBuilder不保证线程安全，在没有多线程冲突的情况下使用StringBuilder会有更好的性能。

### List和Set的区别
> - List是一种顺序集合，可以通过下标对List集合内的元素进行操作，和Set不同，List内的元素是可以重复的。
> - Set是一种不包含重复元素的集合，其中最多包含一个null元素。

### Java的异常
所有的异常都是由Throwable继承而来，在下一层分解为Error和Exception。
> - Error类层次结构描述了Java运行时系统的内部错误和资源耗尽错误，如OutOfMemoryError和StackOverflowError。
> - Exception分为RuntimeException和其他异常。RuntimeException包括：错误的强制类型转换，数组访问越界ArrayIndexOutOfBoundsException，访问null指针NullPointerException。

派生于Error类和RuntimeException类的所有异常都为非检查型异常，所有其他异常称为检查型异常。
### <a name="Title.02">二、对象与类</a>
### Java面向对象的三个特征与含义？
> 面向对象的程序是由对象组成的，每个对象包含对用户公开的特定功能部分和隐藏的实现部分，面向对象首先考虑对象数据再考虑方法。
- **继承**：**继承是从已有类得到继承信息创建新类的过程。** 提供继承信息的类被称为父类（超类、基类）；得到继承信息的类被称为子类（派生类）。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段。继承中最常使用的两个关键字是extends（用于基本类和抽象类）和implements（用于接口）。**Java中类的继承是单一继承，若使用extends只允许有一个父类，使用implements则不限。**

- **封装**：**通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。** 面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口。

> 四种访问控制符：
>1. 默认的，也称为`default`，在同一包内可见，不使用任何修饰符。
>2. 私有的，以`private`修饰符指定，在同一类内可见。
>3. 公有的，以`public`修饰符指定，对所有类可见。
>4. 受保护的，以`protected`修饰符指定，对同一包内的类和所有子类可见。

- **多态**：**多态性是指允许不同子类型的对象对同一消息作出不同的响应。** 简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。**方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。**
> - 关于重写，虚拟机编译期间会根据静态类型将对应的方法引用写入class中，运行时，JVM会根据INVOKEVIRTUAL所指向的方法引用在常量池找到该方法的偏移量，并根据this指针找到实际指向的对象，访问这个对象类型的方法表，根据偏移量找出存放目标方法引用的位置，取出这个引用，调用这个引用实际指向的方法，完成多态！



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

### 反射机制
能够分析类能力的程序称为反射，反射可以用来在运行时分析类，运行时检查对象，实现泛型数组操作代码，利用Method对象调用任意方法。
> - 分析类，java.lang.reflect包中有三个类Field、Method和Constructor分别用来描述类的字段、方法和构造器，可以使用getname方法获得他们的名字，getType获得类型，getModifiers获得修饰符。Class类中的getFields、getMethods和getConstructors方法可以返回这个类支持的公共字段、方法和构造器的数组，其中包括超类的公共成员。
> - 编写泛型数组代码，java.lang.reflect中Array类的静态方法newInstance可以用来构造一个新数组，只需要提供两个参数，数组元素类型和数组的长度。
> - 调用任意方法和构造器，Method类的invoke方法允许调用包装在当前Method对象中的方法

### <a name="Title.04">四、接口、lambda表达式与内部类</a>
### 接口与抽象类的区别？
> - 包含一个或多个抽象方法的类必须被声明为抽象的，抽象类中除了抽象方法还可以包含字段和具体方法。抽象方法相当于占位方法，需要在子类中具体实现，如果子类中没有实现全部抽象方法那子类也必须标记为抽象类。抽象类不可以被实例化，如果一个类声明为abstract，就不能创建其对象。
> - 接口不是类，而是对希望符合这个接口的类的一组需求，接口中的所有方法都自动是public方法，不必提供public关键字标识。接口可以包含多个方法，可以定义常量，但是接口中不会有实例字段，Java8之前也不会实现方法，Java8之后可以实现简单方法，但是不能引用实例字段，且必须用default修饰符标记。Java8中允许接口增加静态方法，Java9中接口方法可以使private。在实现接口时，必须把方法声明为public，否则会按类的默认访问属性来标识，编译器报错。
> - 接口和抽象类都可以实现继承，表示一种通用属性，但是两者还是有区别，首先每个类只能扩展一个类，但每个类可以实现多个接口。