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

### 封装与拆箱
为了让基本类型也拥有对象的属性，基本类型和对应的包装类可以相互装换：
- 由基本类型向对应的包装类转换称为装箱，例如把 int 包装成 Integer 类的对象；
- 包装类向对应的基本类型转换称为拆箱，例如把 Integer 类的对象重新简化为 int。

可以通过 Integer 类的构造方法将 int 装箱，通过 Integer 类的 intValue 方法将 Integer 拆箱。Java 1.5 之后可以自动拆箱装箱，也就是在进行基本数据类型和对应的包装类转换时，系统将自动进行。

### stream流的数组转化
> ### 一. int[ ]转化
> ### 1.1、int[ ] 转 List< Integer >
> ```java 
>public static void main(String[] args) {
>        int[] arr = { 1, 2, 3, 4, 5 };
>       List<Integer> list = Arrays.stream(arr).boxed().collect(Collectors.toList());
>        list.forEach(e -> System.out.print(e + " "));
>    }
> ```
> - Arrays.stream(arr) 将int数组转化为IntStream
> - boxed() 将每一个整数进行装箱，把IntStream 转换成了 Stream< Integer >
> - collect(Collectors.toList()) 将对象流收集为集合，转化为 List< Integer >
> ### 1.2、int[ ] 转 Integer[ ]
> ```java
> Integer[] integers = Arrays.stream(arr).boxed().toArray(Integer[]::new);
> ```
> - Arrays.stream(arr) 还是转化为流
> - boxed() 装箱，将基本类型流转换为对象流
> - toArray(Integer[ ]::new) 将对象流转换为对象数组
> ### 二、Integer[ ]
> ### 2.1、Integer[ ]转 int[ ]
> ```java
> int[] arr= Arrays.stream(integers).mapToInt(Integer::valueOf).toArray();
> ```
> - mapToInt(Integer::valueOf) 将对象流转化为基本类型流
> - toArray() 转化为int数组
> ### 2.2、Integer[ ]转 List<Integer>
> ```
> Integer[] integers = {1,2,3,4,5};
> List<Integer> list = Arrays.asList(integers); 
> ```
> ### 三、List< Integer >
> ### 3.1、List< Integer > 转 Integer[ ]
> ```
> Integer[] integers = list.toArray(new Integer[list.size()]);
> ```
> ### 3.2、List< Integer > 转 int[ ]
> ```
> int[] arr2 = list.stream().mapToInt(Integer::valueOf).toArray();
> ```
> .
### static关键词
- 如果将一个字段定义为static，那么每个类只有一个这样的字段，而对于非静态的实例字段，每个对象有一个自己的副本。
- 静态常量
- 静态方法，static关键字修饰的方法，静态方法不是在对象上执行的方法，可以认为静态方法没有this参数，静态方法只能访问类的静态字段。
> - 静态方法不能重写，静态方法的调用在字节码中是使用INVOKESTATIC，此方法则是直接调用方法区中静态方法，无需经过方法表，因此静态方法的执行只看静态类型，而与实际类型无关，又因为重写的方法调用看的是实际类型，所以静态方法不能被重写。

### final关键词
> ### fianl的使用场景
> - 利用final修饰实例字段：这样的字段必须在构造对象时初始化，并且以后不能再修改这个字段。
> - 利用final修饰类：定义类时使用final修饰，final类不允许被扩展，final类中的所有方法自动地称为final方法。String类是final类。
> - 利用final修饰方法：类中的某个方法可以被声明为final，子类将不能覆盖这个方法。
> - 利用final修饰方法参数：参数在方法局部作用域内不可更改。
> ### final的内存语义
> ### 1、final域的重排序规则
> 1. 在构造函数内对一个final域的写入，与随后把这个被构造对象的引用赋值给一个引用变量，这两个操作之间不能重排序。
> 2. 初次读一个包含final域的对象的引用，与随后初次读这个final域，这两个操作之间不能重排序。
> ### 2、写final域的重排序规则
> 写final域的重排序规则禁止把final域的写重排序到构造函数之外。这个规则的实现包含下面2个方面：
> 1. JMM禁止编译器把final域的写重排序到构造函数之外。
> 2. 编译器会在final域的写之后，构造函数return之前，插入一个StoreStore屏障。这个屏障禁止处理器把final域的写重排序到构造函数之外。写final域的重排序规则可以确保：在对象引用为任意线程可见之前，对象的final域已经被正确初始化过了，而普通域不具有这个保障。
> ### 3、读final域的重排序规则
> **读final域的重排序规则是，在一个线程中，初次读对象引用与初次读该对象包含的final域，JMM禁止处理器重排序这两个操作**（注意，这个规则仅仅针对处理器）。编译器会在读final域操作的前面插入一个LoadLoad屏障。初次读对象引用与初次读该对象包含的final域，这两个操作之间存在间接依赖关系。由于编译器遵守间接依赖关系，因此编译器不会重排序这两个操作。大多数处理器也会遵守间接依赖，也不会重排序这两个操作。但有少数处理器允许对存在间接依赖关系的操作做重排序（比如alpha处理器），这个规则就是专门用来针对这种处理器的。**读final域的重排序规则可以确保：在读一个对象的final域之前，一定会先读包含这个final域的对象的引用。**
> ### 4、final域为引用类型
> 对于引用类型，写final域的重排序规则对编译器和处理器增加了如下约束：在构造函数内对一个final引用的对象的成员域的写入，与随后在构造函数外把这个被构造对象的引用赋值给一个引用变量，这两个操作之间不能重排序。**这一规则确保了其他线程能读到被正确初始化的final引用对象的成员域。**

### try-catch-finally的使用
1. 不管有木有出现异常，finally块中代码都会执行；
2. 当try和catch中有return时，finally仍然会执行；
3. finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，管finally中的代码怎么样，返回的值都不会改变，任然是之前保存的值），所以函数返回值是在finally执行前确定的；
4. finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值。

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
> Object类中的公有函数主要有`toString()`, `hashCode()`, `equals()`, `wait()`, `notify()`, `notifyAll()`, `getClass()`, `clone()`, `finalize()`
### <a name="JO.01">toString()</a>
用于显示调用输出对象信息，或者`this + "string"`字符串重载`+`运算符形式，将`this`转为`String`类型（隐式调用）。  
  
### <a name="JO.02">hashCode()</a>
用于`HashMap`中元素增删改查时`Key`的`Hash`操作。JDK8 的默认hashCode的计算方法是通过和当前线程有关的一个随机数+三个确定值，运用Marsaglia's xorshift scheme随机数算法得到的一个随机数，还有其余几种计算hashCode计算方式，可以通过 -XX:hashCode=4 来设置。一个类重写了equals方法，则必须重写hashCode()方法，因为hashCode()需要保证equals()相等的两个对象的哈希值相同，否则基于hash的容器使用会出问题,JDK`HashMap`的`hash()`源码如下:
  
```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
  
重写`hashCode()`函数是一个考点，需要注意一些细节。重写`hashCode()`函数
  
```java
public int hashCode() {
	return id != null ? id.hashCode() : 0;
    // 或者自定义Hash算法
}
```
  
### <a name="JO.03">equals()</a>
用于对象相等测试，比如容器`indexOf()`、`remove()`、`contains()`等函数中,JDK`ArrayList`的`indexOf()`源码如下
  
```java
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
  
#### "=="和equals()的区别
对于基本类型和引用类型 == 的作用效果是不同的，如下所示：
- 基本类型：比较的是值是否相同；
- 引用类型：比较的是引用是否相同；

Object类中的`equals()`等价于==，不过许多类中的`equals()`方法会重写`equals()`，进行类型检查并作值相等判断。

重写`equals()`函数是一个考点，需要注意一些细节,重写`equals()`函数
  
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