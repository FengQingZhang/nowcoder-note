# 2021.12.29

## 构造方法

Java会给没有创建任何构造函数的类创建一个不带参数的构造函数

## 覆盖

方法的覆盖，也叫方法的重写。重写的前提，必须有继承。开发中，子类重写父类的方法，直接将父类被重写方法从权限修饰符到第一个花括号，直接复制粘贴，方法体改写即可。此时想要调用父类的被重写方法，需要用到super关键字。

## Java Thread run和start方法的区别

- run方法是线程内重写的一个方法
- 调用start方法时，才是启动了一个线程，线程此时为就绪状态。一旦获得CPU时间片，此时jvm会自动去调用相应的run方法，进入运行状态。

## Java程序执行顺序

1. 父类静态代码块，如果有多个静态代码块，按顺序执行，仅执行一遍
2. 子类静态代码块
3. 父类非静态代码块，按顺序执行，且每次执行
4. 父类构造函数。
5. 子类非静态代码块：有多个非静态代码块，按顺序执行，且每次new，每次执行
6. 子类构造函数

## Thread.sleep和wait异常

Thread.sleep()和Object.wait()都会抛出interruptedException,而wait是必须要在synchronized内使用的，wait的本意是暂时释放掉对象锁。

## Semaphore 、CyclicBarrier、CountDownLatch

![](img/同步器.jpg)

### 1、Semaphore

#### 1.1、简介

Semaphore 字面意思是信号量的意思，它的作用是控制访问特定资源的线程数目

#### 1.2、方法

##### 1.2.1、构造方法

```java
public Semaphore(int permits);//permits表示许可线程的数量
public Semaphore(int permits,boolean fair);//fair表示公平性，如果这个设为true的话，下次执行的线程会是等待最久的线程
```

##### 1.2.2、重要方法

```java
public void acquire() throws InterruptedException//表示阻塞并获取许可
public void release()//表示释放许可
```

#### 1.3、使用场景

可用于流量控制，限制最大的并发访问数。

### 2、CyclicBarrier

#### 2.1简介

JDK1.5开始提供的并发编程，辅助工具类。用于并发编程。字面意思是回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行。叫做回环是因为当所有等待线程都被释放后，cyclicBarrier可以被重用。叫做栅栏，大概是描述所有线程被栅栏挡住了，当都达到时，一起跳过栅栏执行，也算形象，我们可以把这个状态就叫做barrier。

#### 2.2、方法

##### 2.2.1、构造方法

```java
public CyclicBarrier(int parties)
/**
第一个参数，表示一起执行的线程个数，第二参数，表示线程处于barrier时，一起执行之前，先执行的一个线程
**/
public CyclicBarrier(int parties, Runnable barrierAction)
```

##### 2.2.2、重要方法

让线程处于barrier状态的方法await()

```java
public int await()
public int await(long timeout, TimeUnit unit)
```

第一个默认方法，表示要等到所有的线程都处于barrier状态，才一起执行
第二个方法，指定了等待的时间，当所有线程没有都处于barrier状态，又到了指定的时间，所在的线程就继续执行了。

#### 3、底层原理

CyclicBarrier类是concurrent并发包下的一工具类。
 线程间同步阻塞是使用的是ReentrantLock，可重入锁
 线程间通信使用的是Condition，Condition 将 Object 监视器方法（wait、notify 和 notifyAll）分解成截然不同的对象，以便通过将这些对象与任意 Lock 实现组合使用。

### 3、CountDownLatch

#### 3.1、简介

​	countDown是倒计时的意思，Latch是门栓的意思，加起来的意思就是一个倒计时的门栓，它其实是作用于线程当中的，它就像一个门栓，一开始是关闭的，所有希望通过该门的线程都需要等待，然后开始倒计时，当倒计时一到，等待的所有线程都可以通过。

要注意的是，它是一次性的，打开之后就不能关上了。

#### 3.2、构造方法

```java
public CountDownLatch(int count)//count就是需要等待的线程数量
```

#### 3.3、重要方法

```java
// 调用此方法的线程会被阻塞，直到 CountDownLatch 的 count 为 0
public void await() throws InterruptedException 

// 和上面的 await() 作用基本一致，只是可以设置一个最长等待时间
public boolean await(long timeout, TimeUnit unit) throws InterruptedException

// 会将 count 减 1，直至为 0
public void countDown() 
```

# 2021.12.30

## List、Set、Map

List,Set继承了Collection接口，而Map是一个单独的接口； 

  List的子类有ArrayList、LinedList和Vector：LinedList和Vector的底层都是基于链表，而ArrayList的底层是基于线性表。LinedList和ArrayList的共同点是线程是不安全的，不同点是ArrayList在做增删元素的时候，效率高，查询效率低；linedList恰好相反；这和它们的底层有关系。 

  Set的实现类HashSet,TreeSet。这俩个类都保存了元素的唯一性，但是hashSet的元素是无需的，而TreeSet的元素是有序的。 

  Map的实现类：HashMap,HashTable它们的区别是HashMap的key可以为null，而HashTable的key不能为Null; 
  当往Map集合中放入相同的key时，前者的key会覆盖后者的key。

## 抽象类和接口不能实例化

## 泛型

泛型仅仅是java的语法糖，他不会影响java虚拟机生成的汇编代码，在编译阶段，虚拟机就会把泛型的类型擦除，还原成没有泛型的代码，顶多编译速度稍微慢一些，执行速度是完成没有什么区别的，

## 类型自动转换规则

### 概述

自动类型转换也叫隐式类型转换

### 规则

1. 若参与运算的数据类不同，则先转换成同一个类型，然后进行运算，

2. 转换按数据长度增加的方向进行，以保证精度不降低。如果一个操作数是long型，计算结果就是long型；如果一个操作数是float型，计算结果就是float型；如果一个操作数是double型，计算结果就是double型。例如int型和long型运算时，先把int量转成long型后再进行运算。

3. 所有的浮点运算都是以双精度进行的，即使仅含float单精度量运算的表达式，也要先转换成double型，再作运算。

4. char型和short型参与运算时，必须先转换成int型。

5. 在赋值运算中，赋值号两边的数据类型不同时，需要把右边表达式的类型将转换为左边变量的类型。如果右边表达式的数据类型长度比左边长时，将丢失一部分数据，这样会降低精度。

   ![](img/类型自动转换的规则.png)

   

### 数据类型只会自动提升，不能自动降低

int值可以赋值给long，float，double型变量，不能赋值给byte、short、char型变量

```java
int a =66;
//没报错
long b = a;
float c = a;
double d = a;
//报错
byte e = a;
short f = a;
char g =a;
```

### Java中整数默认的数据类型是int类型

所有长度低于int的类型（byte、short、char）在运算之后结果将会被提升为int型

## java锁的种类

http://ifeve.com/java_lock_see/

# 2021.12.31

## 事务隔离

​	在操作系统中，为了有效并发读取数据的正确性，提出的事务隔离级别；为了解决更新丢失，脏读，不可重读（包括虚读和幻读）等问题，在标准SQL规范中，定义了4个事务隔离级别，分别为未授权读取，也称为读未提交（read uncommitted）；授权读取，也称为读提交（read committed）;可重复读取（repeatable read）；序列化（Serializable）

|                  | 脏读 | 不可重复读 | 幻读 |
| ---------------- | ---- | ---------- | ---- |
| read uncommitted | √    | √          | √    |
| read committed   | ×    | √          | √    |
| repeatable read  | ×    | ×          | √    |
| serializable     | ×    | ×          | ×    |

## HashMap

Map是键值对集合。其中key列就是一个集合，key不能重复，但是value可以重复。HashMap、TreeMap和Hashtable是Map的三个主要的实现类。HashTable是线程安全的，不能存储null值；HashMap不是线程安全的，可以存储null值。

## 默认类型

接口中的变量默认是public static final的，方法默认是public abstract的

## 表达式数据类型自动提升

1. 所有的byte,short,char类型的值将提升为int型；
2. 如果有一个操作数是long型，计算结果是long型；
3. 如果有一个操作数是float型，计算结果是float型；
4. 如果有一个操作数是double型，计算结果是double型；
5. 而声明为final的变量，不会被转换。

## Queue

### LinkedBlockingQueue

基于链接节点的可选限定的blocking queue。这个队列元素FIFO（先进先出）。队列的头部是队列中最长的元素。队列的尾部是队列中最短时间的元素。新元素插入队列的尾部，队列检索操作获取队列头部的元素。链接队列通常具有比基于阵列的队列更高的吞吐量，但在大多数并发应用程序中的可预测性能较低。（ps：blocking queue说明：不接受null元素；可能是容量有限的；实现被设计为主要用于生产者-消费者队列；不支持任何类型的“关闭”或者“关闭”操作，表示不再添加项目实现是线程安全的）

### PriorityQueue

1. 基于优先级堆的无限优先级queue。优先级队列的元素根据它们的有序natural ordering，或由一个Comparator在队列构造的时候提供。这取决于所使用的构造方法。优先队列不允许null元素。依靠自然排序的优先级队列也不允许插入不可比较的对象（这样做可能导致ClassCastException）
2. 该队列的头部是想对于顺序的最小元素。如果多个元素被绑定到最小值，那么头就是这些元素之一，关系被破坏。队列检索操作poll，remove，peek和element访问在队列的头部的元件。
3. 优先级队列是无限制的，但是具有管理用于在队列上存储元素的数组的大小的内部容量。它始终至少与队列大小一样大。当元素被添加到优先级队列中时，其容量会自动增长。没有规定增长政策的细节。
4. 该类及其迭代器实现Collection和Iterator接口的所有可选方法。 方法iterator()中提供的迭代器不能保证以任何特定顺序遍历优先级队列的元素。 如果需要有序遍历，请考虑使用Arrays.sort(pq.toArray()) 。
5. 请注意，此实现不同步。 如果任何线程修改队列，多线程不应同时访问PriorityQueue实例。 而是使用线程安全的PriorityBlockingQueue类。
6. 实现注意事项：此实现提供了O（log(n)）的时间入队和出队方法（ offer ， poll ， remove()和add ）; remove(Object)和contains(Object)方法的线性时间; 和恒定时间检索方法（ peek ， element和size ）。

### ConcurrentLinkedQueue

一个基于链接节点的无界线程安全队列(双端队列)，它采用先进先出的规则对节点进行排序，当我们添加一个元素的时候，它会添加到队列的尾部，当我们获取一个元素时，它会返回队列头部的元素。是许多线程将共享对公共集合的访问的适当选择。像大多数其他并发集合实现一样，此类不允许使用null元素。

## 流

### 分类

按照流是否直接与特定的地方（如磁盘、内存设备等）相连，分为节点流和处理流两类。

节点流：可以从或向一个特定的地方（节点）读写数据。如FileRead二

处理流：是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。如bufferedReader处理流的构造方法总是要带一个其他的流对象做参数，一个流对象经过其他流的多次包装，称为流的链接。

### 节点流

#### 文件

FileInputStream FileOutputStream FileReader FileWriter 文件进行处理的节点流。

#### 字符串

StringReader StringWrite对字符串进行处理的节点流

#### 数组

ByteArrayInputStream ByteArrayOutputStream CharArrayReader CharArrayWriter 对数组进行处理的节点流（对应的不再是文件，而是内存中的一个数组）。

#### 管道

PipedInputStream PipedOutputStream 

PipedReader PipedWriter 对管道进行处理节点流

### 处理流

#### 缓冲流

BufferedInputStream BufferedOutputStream BufferedReader BufferedWriter 增加缓冲功能，避免频繁读写硬盘

#### 转换流

inputStreamReader outputStreamReader 实现字节流和字符流之间的转换。

#### 数据流

DataInputStream DataOutputStream等提供将基础数据类型写入到文件中，或者读取处理。

### 流的关闭

1. 一般情况下是：先打开的后关闭，后打开的先关闭。
2. 另一种情况：看依赖关系，如果流a依赖流b，应该先关闭流a，再关闭流b。例如，处理流a依赖节点流b，应先a后b。
3. 可以只关闭处理流，不用关闭节点流。处理流关闭的时候，会调用其处理的节点流的关闭方法。

# 2022.1.1

## Java基本类型

| 变量名称 | 字节 | 位数 |
| -------- | ---- | ---- |
| byte     | 1    | 8    |
| short    | 2    | 16   |
| int      | 4    | 32   |
| long     | 8    | 64   |
| float    | 4    | 32   |
| double   | 8    | 64   |
| char     | 2    | 16   |
| boolean  | 1    | 8    |

## 序列化

java在序列化时不hi实例化static变量和transient修饰的变量，因为static代表类的成员，transient代表对象的临时数据，被声明这种类型的数据成员不能被序列化，

## 调用子类构造方法

调用子类构造方法时，必须先调用父类构造方法，由于父类直接书写了构造方法，所以子类构造方法不能调用父类的默认构造方法，因此编译错误。

# 2022.1.2

## 接口可以多继承

## 修饰符

### private

只能在类内被访问

### 无修饰符

即使default能被这个类和所在包中的类访问

### public

任何类都可以访问

### protected 

能被这个类和包中的其他类还有他的子类访问

## ServletContext与ServletConfig

ServletContext对象：servlet容器在启动时会加载web应用，并为每个web应用创建唯一的servlet context对象，可以把ServletContext看成是一个Web应用的服务器端组件的共享内存，在ServletContext中可以存放共享数据。ServletContext对象是真正的一个全局对象，凡是web容器中的Servlet都可以访问。


     整个web应用只有唯一的一个ServletContext对象


  servletConfig对象：用于封装servlet的配置信息。从一个servlet被实例化后，对任何客户端在任何时候访问有效，但仅对servlet自身有效，一个servlet的ServletConfig对象不能被另一个servlet访问。

## 子类引用父类的静态字段

子类引用父类的静态字段，只会触发子类的加载、父类的初始化、不会导致子类初始化。而静态代码块在类初始化的时候执行。

# 2022.1.3

## 正则表达式

| \d   | 匹配一个数字字符。等价于[0-9]                                |
| ---- | ------------------------------------------------------------ |
| \D   | 匹配一个非数字字符。等价于【^0-9】                           |
| \f   | 匹配一个换页符，等价于\x0c和\cL                              |
| \n   | 匹配一个换行符，等价于\x0a和\cJ                              |
| \r   | 匹配一个回车符，等价于\x0d和\cM                              |
| \s   | 匹配任何空白字符。等价于[\f\n\r\t\v].                        |
| \S   | 匹配任何非空白字符。等价于匹配任何空白字符。等价于【^\f\n\r\t\v】 |
| \t   | 匹配一个制表符。等价于\x09和\cl                              |
| \v   | 匹配一个垂直制表符，等价于\x0b和\cK.                         |
| \w   | 匹配字母、数字、下划线。等价于【A-Za-z0-9_】                 |
| \W   | 匹配非字母、数字、下划线。等价于【A-Za-z0-9_】               |
|      |                                                              |

## 管道通信

管道通信类似于通信中半双通道的进程通信机制，一个管道可以实现双向的数据传输，而同一个时刻只能最多有一个方向的传输，不能两个方向同时进行，

## 代码优化

常见的代码优化技术有：复写传播，删除死代码，强度削弱，归纳变量删除。

# 2022.1.4

## static

static表示静态变量，归类所有，该类的所有对象公用。

## Object的方法

### 1．clone方法 

  保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常。 

###   2．getClass方法 

  final方法，获得运行时类型。 

###   3．toString方法 

  该方法用得比较多，一般子类都有覆盖。 

###   4．finalize方法 

  该方法用于释放资源。因为无法确定该方法什么时候被调用，很少使用。 

###   5．equals方法 

  该方法是非常重要的一个方法。一般equals和==是不一样的，但是在Object中两者是一样的。子类一般都要重写这个方法。 

###   6．hashCode方法 

  该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到。 

  一般必须满足obj1.equals(obj2)==true。可以推出obj1.hash-
  Code()==obj2.hashCode()，但是hashCode相等不一定就满足equals。不过为了提高效率，应该尽量使上面两个条件接近等价。 

###   7．wait方法 

  wait方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait()方法一直等待，直到获得锁或者被中断。wait(long
  timeout)设定一个超时间隔，如果在规定时间内没有获得锁就返回。 
  调用该方法后当前线程进入睡眠状态，直到以下事件发生。 
  （1）其他线程调用了该对象的notify方法。 
  （2）其他线程调用了该对象的notifyAll方法。 
  （3）其他线程调用了interrupt中断该线程。 
  （4）时间间隔到了。 

  此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。 

###   8．notify方法 

  该方法唤醒在该对象上等待的某个线程。 

###   9．notifyAll方法 

  该方法唤醒在该对象上等待的所有线程。

## synchronized和volatile

synchronized保证三性，原子性，有序性，可见性，

volatile保证有序性，可见性。不能保证原子性。

## ThreadLocal

ThreadLocal用于创建线程的本地变量，该变量是线程之间不共享的。

## 运行内存分为线程共享线程私有

#### 私有：

java虚拟机栈，程序计数器，本地方法栈

#### 共享：

java堆，方法区

## String与StringBuffer

String和StringBuffer这两个类都被final修饰，说的是不可以被继承final修饰的对象或者引用类型，地址值不变，对象属性可变，其次。
