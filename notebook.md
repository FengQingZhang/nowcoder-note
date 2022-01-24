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

| 变量名称 | 字节 | 位数 | 长度     |
| -------- | ---- | ---- | -------- |
| byte     | 1    | 8    | -128-127 |
| short    | 2    | 16   | -256-255 |
| int      | 4    | 32   |          |
| long     | 8    | 64   |          |
| float    | 4    | 32   |          |
| double   | 8    | 64   |          |
| char     | 2    | 16   |          |
| boolean  | 1    | 8    |          |

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

# 2022.1.5

## java的堆内存

简单来说java的堆内存分为两块：permantspace(持久带)和heapspace.

持久带中主要存放用于存放静态类型数据，如Java Class，Method等，与垃圾收集器要收集的java对象关系不大。

而headspace分为年轻带和老年带

年轻代的垃圾回收叫Young GC（ps：对象被创建时，内存的分配首先发生在年轻代（大对象可以直接被创建在年老代），大部分的对象在创建后很快就不再使用，因此很快变得不可达，于是被年轻代的GC机制清理掉（IBM的研究表明，98%的对象都是很快消亡的），这个GC机制被称为Minor GC或叫Young GC。注意，Minor GC并不代表年轻代内存不足，它事实上只表示在Eden区上的GC），

年老代的垃圾回收叫Full GC（ps：对象如果在年轻代存活了足够长的时间而没有被清理掉（即在几次Young GC后存活了下来），则会被复制到年老代，年老代的空间一般比年轻代大，能存放更多的对象，在年老代上发生的GC次数也比年轻代少。当年老代内存不足时，将执行Major GC，也叫 Full GC。）

在年轻代中经历了N次（可配置）垃圾回收后仍然存活的对象，就会被复制到年老代中，因此可以以为年老带中存放的都是一些生命周期较长的对象。

年老代溢出原因有循环上万次的字符串处理、创建上千万个对象，在一端代码内申请上百M甚至上G的内存

持久带溢出原因动态加载了大量java类而导致溢出

## 抽象方法的访问权限

以前抽象类或者抽象方法默认是protected，jdk1.8以后改成默认default。

## 访问权限

局部内部类前不能用修饰符public和private，protected

内部类随意。

# 2022.1.6

## 二分法

### 模板一

用于查找数组中的单个索引来确定的元素或条件。

**关键属性**

- 二分查找的最基础和最基本的形式。
- 查找条件可以在不与元素的两侧进行比较的情况下确定（或使用它周围的特定元素）
- 不需要后处理，因为每一步中，你都在检查是否找到了元素。如果达到末尾，则知道未找到该元素。

**区分语法**

- 初始条件：left = 0，right=length-1；
- 终止： left > right;
- 向左查找：right = mid-1;
- 向右查找  left=mid+1;

```java
int binarySearch(int[] nums,int target){
    if(nums==nullnums.length==0){
        return -1;
    }
    int left=0,right=nums.length-1;
    while(left<=right){
        int mid = left+(right-left)/2;
        if(num[mid]==target){
            return mid;
        }else if(num[mid]<target){
            left = mid+1;
        }else if(num[mid]>target){
            right = mid-1;
        }
    }
    return -1;
}
```

### 模板二

用于查找需要访问数组中当前索引及其直接右邻居索引的元素或条件，

**关键属性**

- 一种实现二分查找的高级方法。
- 查找条件需要访问元素的直接右邻居。
- 使用元素的右邻居老确定是否满足条件，并决定是向左还是向右。
- 保证查找空间在每一步中至少有2个元素。
- 需要进行后处理。当你剩下1个元素时，循环/递归结束。需要评估剩余元素是否符合条件。

**区分语法**

- 初始条件：left = 0, right =length
- 终止：left == right
- 向左查找； right = mid；
- 向右查找：left = mid+1;

```java
int binarySeach(int nums,int target){
    if(nums==null||nums.length==0){
        return -1;
    }
    int left =0,right=nums.length;
    while(left<right){
        int mid = left+(right-left)/2;
        if(nums[mid]==target){
            return mid;
        }else if(nums[mid]<target){
            left = mid+1;
        }else {
            right = mid;
        }
    }
    if(left!=nums.length&&nums[left]==target)
        return left;
    return -1;
}
```

### 模板三

​	用于搜索需要访问当前索引及其在数组中的直接左右邻居索引的元素或条件。

**关键属性**

- 实现二分查找的另一种方法。

- 搜索条件需要访问元素的直接左右邻居。
- 使用元素的邻居来确定它是向右还是向左。
- 保证查找空间在每个步骤中至少有 3 个元素。
- 需要进行后处理。 当剩下 2 个元素时，循环 / 递归结束。 需要评估其余元素是否符合条件。

**区分语法**

- 初始条件：left = 0, right = length-1

- 终止：left + 1 == right
- 向左查找：right = mid
- 向右查找：left = mid

```java
int binarySearch(int[] nums, int target) {
    if (nums == null || nums.length == 0)
        return -1;

    int left = 0, right = nums.length - 1;
    while (left + 1 < right){
        // Prevent (left + right) overflow
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid;
        } else {
            right = mid;
        }
    }

    // Post-processing:
    // End Condition: left + 1 == right
    if(nums[left] == target) return left;
    if(nums[right] == target) return right;
    return -1;
}
```

### 总结

left + 1 < right；因为出来的时候是left + 1 = right 所以保证了两个元素；left和right都能用；排序数组中查找元素的第一个和最后一个位置
left < right 则保证了一个元素；可以用left/right
left <= right 则直接一个元素都没有；只能在while里mid赋值

# 2022.1.7

## 线程通知和唤醒

wait()、notify()和notifyAll()是Object类中的方法；Condition是在java1.5中才出现的，它用来替代传统的Object的wait(),notify()实现线程间的协作，相比使用Object的wait()、notify(),使用Condition的await()、signal()这种方式实现线程间协作更加安全和高效；

## java编译和运行命令

编译java文件->使用javac 文件名.java

执行使用 ->java 文件名

## java内存区域

### 程序计数器

程序计数器是一块较小的内存空间，它的作用可以看做是当前线程所执行的字节码的信号指示器（偏移地址）。java编译过程中产生的字节码有点类似编译原理的指令，程序计数器（程序计数器的内存空间是线程私有的），因为当执行语句时，改变的是程序计数器的内存空间，因此它不会发送内存溢出，并且程序计数器是jvm虚拟机规范中唯一个没有规定OutOfMemoryError异常的区域；

### Java虚拟机栈

线程私有。生命周期和线程一致。描述的是Java方法执行的内存模型；每个方法在执行时都会创建一个栈帧（Stack Frame）用于存储局部变量表、操作数栈，动态链接，方法出口等信息。每一个方法从调用直至执行结束，就对应着一个栈帧从虚拟机栈中入栈到出栈的过程。没有类信息，类信息是在方法区中。

### JAVA堆

对于绝大数应用来说，这块区域是JVM所管理的内存中最大的一块。线程共享，主要是存放对象实例和数组

### 方法区

属于共享内存区域，存储已被虚拟机加载的类的信息，常量、静态变量、即时编译器编译后的代码等数据

# 2022.1.8

## 识别合法的构造方法

1. 构造方法可以被重载，一个构造方法可以通过this关键字调用另一个构造方法，this语句必须位于构造方的第一行；
2. 当一个类中没有定义任何构造方法，java将自动提供一个缺省构造方法。
3. 子类通过super关键字调用父类的一个构造方法。
4. 当子类的某个构造方法没有通过super关键字调用父类的构造方法，通过这个构造方法创建子类对象时，会自动先调用父类的缺省构造方法。
5. 构造方法不能被static，final、synchronized、abstract、native修饰，但是可以被public、private、protected修饰；
6. 构造方不是类的成员方法。
7. 构造方法不能被继承

## 执行顺序

静态代码块>main() >构造代码块>构造方法

## Final关键字

1. final修饰变量，则等同于常量
2. final修饰方法中的参数，称为最终参数，
3. final修饰类，则类不能被继承
4. final修饰方法，则方法不能被重写，可以重载
5. 不能修饰抽象类

# 2022.1.9

## 值传递与引用传递

**值传递**：是指在调用函数时，将实际参数复制一份传递给函数，这样在函数中修改参数时，不会影响到实际参数。其实，就是在说值传递时，只会改变形参，不会改变实参。

**引用传递**：是指在调用函数时，将实际参数的地址传递给函数，这样在函数中对参数的修改，将影响到实际参数

1）形参为基本类型时，对形参的处理不会影响实参。   

 2）形参为引用类型时，对形参的处理会影响实参。    

3）String,Integer,Double等immutable类型的特殊处理，可以理解为值传递，形参操作不会影响实参对象。

## 基本类型的默认值和取值范围

|         | 默认值   | 存储需求（字节） | 取值范围       | 示例            |
| ------- | -------- | ---------------- | -------------- | --------------- |
| byte    | 0        | 1                | -2^7^~2^7^-1   |                 |
| char    | '\u0000' | 2                | 0~2^16^-1      |                 |
| short   | 0        | 2                | -2^15^~2^15^-1 |                 |
| int     | 0        | 4                | -2^31^~2^31^-1 |                 |
| long    | 0        | 8                | -2^63~^2^63^-1 | long o=10L;;    |
| float   | 0.0f     | 4                | -2^31^~2^31^-1 | float f =10.0F; |
| double  | 0.0d     | 8                | -2^63^-2^63^-1 | double b =10.0; |
| boolean | false    | 1                | false/true     |                 |

## Servlet

servlet是线程不安全的，在Servlet类中可能会定义共享的类变量，这样在并发的多线程访问的情况下，不同的线程对成员变量的修改会引发错误。

**init方法**： 是在servlet实例创建时调用的方法，用于创建或打开任何与servlet相的资源和初始 化servlet的状态，Servlet规范保证调用init方法前不会处理任何请求  
   **service方法**：是servlet真正处理客户端传过来的请求的方法，由web容器调用，
  根据HTTP请求方法（GET、POST等），将请求分发到doGet、doPost等方法  
  **destory方法**：是在servlet实例被销毁时由web容器调用。Servlet规范确保在destroy方法调用之前所有请求的处理均完成，需要覆盖destroy方法的情况：释放任何在init方法中 打开的与servlet相关的资源存储servlet的状态

## 四类八种基本类型

### 1、整数类型

byte short int long

### 2、浮点型

float double

### 3、逻辑型

boolean

### 4、字符型

char

## 类的加载过程

1.  JVM会先去方法区中找有没有相应类的.class存在。如果有，就直接使用；如果没有，则把相关类的.class加载到方法区 
2. 在.class加载到方法区时，会分为两部分加载：先加载非静态内容，再加载静态内容 
3.  加载非静态内容：把.class中的所有非静态内容加载到方法区下的非静态区域内 
4.  加载静态内容： 
     4.1、把.class中的所有静态内容加载到方法区下的静态区域内 
     4.2、静态内容加载完成之后，对所有的静态变量进行默认初始化 
     4.3、所有的静态变量默认初始化完成之后，再进行显式初始化 
     4.4、当静态区域下的所有静态变量显式初始化完后，执行静态代码块 
5.  当静态区域下的静态代码块，执行完之后，整个类的加载就完成了。

# 2022.1.10

## 自动拆箱装箱

1. 基本型个基本型封装型进行“==”运算符的比较，基本型封装型将会自动拆箱变为基本型后再进行比较，因此Integer(0)会拆箱为int类型再进行比较，
2. 两个基本型的封装型进行equals()比较，首先equals()会比较类型，如果类型相同，则继续比较值，如果值也相同，返回true
3. 基本型封装类型调用equals(),但是参数是基本类型，这时候会进行自动装箱，基本类型转换为其封装类型。

## java类加载器

1. ### 启动类加载器

  负责将存放在 < JAVA_HOME >\lib目录中的，或者被-Xbootclasspath参数所指定的路径中的，并且是虚拟机识别的（仅按照文件名识别，如rt.jar,名字不符合的类库即使放在lib中也不会被加载）类库加载到虚拟机内存中。 

  2. ### 扩展类加载器

  扩展类加载器（Extension ClassLoader）:这个加载器由sun.misc.Launcher$ExtClassLoader实现，它负责加载<JAVA_HOME>\lib\ext目录中的，或者被java.ext.dirs系统变量所指定的路径中的所有类库，开发者可以直接使用扩展类加载器。 

  3. ### 应用程序类加载器 

  应用程序类加载器(Application ClassLoader):这个类加载器由sun.misc.Launcher$App-ClassLoader实现。由于这个类加载器是ClassLoader中的getSystemClassLoader()方法的返回值，所以一般也称它为系统类加载器。它负责加载用户类路径（classPath）上所指定的类库，开发者可以直接使用这个类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认类加载器。

## import

import 指定的只能是目录下的类，不包括子目录。

# 2022.1.11

## JVM命令

1. jps：查看本机java进程信息
2. jstack：打印线程的栈信息，制作线程dump文件。
3. jmap：打印内存映射，制作堆dump文件
4. jstat: 性能监控工具。
5. jhat 内存分析工具
6. jconsole 简易的可视化控制台
7. jvisualvm 功能强大的控制台
8. jinfo 性能调优工具。

## 垃圾回收

垃圾回收并不是在程序结束时由系统自动回收内存空间，而是在程序运作中，一旦有不在被使用的内存空间，系统就将其回收，以便被新对象使用。

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

## 方法声明

- 抽象方法只可以被public 和 protected修饰；


- final可以修饰类、方法、变量，分别表示：该类不可继承、该方法不能重写、该变量是常量
- static final可以表达在一起来修饰方法，表示是该方法是静态的不可重写的方法
- private 修饰方法（这太常见的）表示私有方法，本类可以访问，外界不能访问

## Map家族的继承

Map家族的继承实现关系如下，注意一点就是顶层的Map接口与Collection接口是依赖关系：

​    ![](img/Map继承关系1.jpg)

​    关于Key和Value能否为null的问题：

![](img/Map继承关系2.jpg)

## ThreadLocal

### 概述

Synchronized用于线程间的数据共享，而ThreadLocal则用于线程之间的数据隔离

ThreadLocal继承Object，相当于没继承任何特殊的。

ThreadLocal没有实现任何接口

ThreadLocal并不是一个Thread，而是Thread的局部变量

### 数据隔离原理

使用一个哈希表来管理，每个线程的变量的副本，从而实现数据在线程的独立。

## forward（转发）和redirect（重定向）

### 简介

**forward（转发）**：

是服务器请求资源,服务器直接访问目标地址的URL,把那个URL的响应内容读取过来,然后把这些内容再发给浏览器.浏览器根本不知道服务器发送的内容从哪里来的,因为这个跳转过程实在服务器实现的，并不是在客户端实现的所以客户端并不知道这个跳转动作，所以它的地址栏还是原来的地址.

**redirect（重定向）**：

是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址.所以地址栏显示的是新的URL.

### 区别

1. 从地址栏显示来说
   forward是服务器请求资源,服务器直接访问目标地址的URL,把那个URL的响应内容读取过来,然后把这些内容再发给浏览器.浏览器根本不知道服务器发送的内容从哪里来的,所以它的地址栏还是原来的地址.

​		redirect是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址.所以地址栏显示的是新的URL.

2. 从数据共享来说
   forward:转发页面和转发到的页面可以共享request里面的数据.
   redirect:不能共享数据.

3. 从运用地方来说
   forward:一般用于用户登陆的时候,根据角色转发到相应的模块.
   redirect:一般用于用户注销登陆时返回主页面和跳转到其它的网站等

4. 从效率来说
   forward:高.
   redirect:低.

# 2022.1.12

## 垃圾回收算法

### Mark-Sweep(标记-清除)算法

这是最基础的垃圾回收算法，之所以说它是最基础的是因为它最容易实现，思想也是最简单的。标记-清除算法分为两个阶段：标记阶段和清除阶段。标记阶段的任务是标记出所有需要被回收的对象，清除阶段就是回收被标记的对象所占用的空间。过程如下所示

![](img/Mark-Sweep.png)

从图中可以很容易看出标记-清除算法实现起来比较容易，但是有一个比较严重的问题就是容易产生内存碎片、碎片太多可能会导致后续过程中需要为大对象分配空间时无法找到足够的空间而提前触发新的一次垃圾收集动作。

### Copying（复制）算法

为了解决Mark-Sweep算法的缺陷，Copying算法就被提了出来，它将可用内存按容量划分为大小相等的两块，每块只使用其中的一块，当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后在把已使用的内存空间一次清理掉，这样一来就不容器出现内存碎片的问题。具体过程如下图所示

![](img/Copying算法.png)

这种算法虽然实现简单，运行高效且不容易产生内存碎片，但是却对内存空间的使用做出了高昂的代价，因为能够使用的内存缩减到原来的一半。很显然，Copying算法的效率跟存活对象的数目多少有很大的关系，如果对象很多，那么Copying算法的效率将会大大降低。

### Mark-Compact(标记-整理)算法（压缩法）

为了解决Copying算法的缺陷，充分利用内存空间，提出了Mark-Compact算法。该算法标记阶段和Mark-Sweep一样，但是在完成标记之后，它不是直接清理可回收对象，而是将存活对象都向一端移动，然后清理掉端边界以外的内存。具体过程如下图所示：

![](img/Mark-Compact(标记-整理)算法（压缩法）.png)

### Generational Collection (分代收集)算法

分代收集算法是目前大部分JVM的垃圾收集器采用的算法。它和核心思想是根据对象存活的生命周期将内存划分为若干个不同的区域。一般情况下将堆区分为老年代（Tenured Generation）和新生代（Young Generation), 老年代的特点是每次垃圾回收时只有少量对象需要被回收，而新生代的特点是每次垃圾回收时都有大量的对象需要被回收，那么就可以根据不同代的特点采取最合适的收集算法。

**目前大部分垃圾收集器对于新生代都采取复制算法**，因为新生代中每次垃圾回收都要回收大部分对象，也就是说需要复制的操作次数较少，但是实际中并不是按照1：1的比例来划分新生代的空间的，一般来说是将新生代划分为一块较大的Eden空间和两块较小的Survivor空间，每次使用Eden空间和其中的一块Survivor空间，当进行回收时，将Eden和Survivor中还存活的对象复制到另一块Survivor空间中，然后清理掉Eden和刚才使用过的Survivor空间。

而由于**老年代的特点是每次回收都只回收少量对象，一般使用的是标记-整理算法（压缩法）**

## 典型的垃圾收集器

垃圾收集算法是 内存回收的理论基础，而垃圾收集器就是内存回收的具体实现。下面介绍一下HotSpot（JDK 7)虚拟机提供的几种垃圾收集器，用户可以根据自己的需求组合出各个年代使用的收集器。

1.Serial/Serial Old收集器 是最基本最古老的收集器，它是一个单线程收集器，并且在它进行垃圾收集时，必须暂停所有用户线程。Serial收集器是针对新生代的收集器，采用的是Copying算法，Serial Old收集器是针对老年代的收集器，采用的是Mark-Compact算法。它的优点是实现简单高效，但是缺点是会给用户带来停顿。

2.ParNew收集器 是Serial收集器的多线程版本，使用多个线程进行垃圾收集。

3.Parallel Scavenge收集器 是一个新生代的多线程收集器（并行收集器），它在回收期间不需要暂停其他用户线程，其采用的是Copying算法，该收集器与前两个收集器有所不同，它主要是为了达到一个可控的吞吐量。

4.Parallel Old收集器 是Parallel Scavenge收集器的老年代版本（并行收集器），使用多线程和Mark-Compact算法。

5.CMS（Concurrent Mark Sweep）收集器 是一种以获取最短回收停顿时间为目标的收集器，它是一种并发收集器，采用的是Mark-Sweep算法。

6.G1收集器 是当今收集器技术发展最前沿的成果，它是一款面向服务端应用的收集器，它能充分利用多CPU、多核环境。因此它是一款并行与并发收集器，并且它能建立可预测的停顿时间模型。

# 2022.1.13

## 编译看左边，运行看右边

父类型引用指向子类型对象，无法调用只在子类型里定义的方法，

## 引用类型的变量也在栈区，只是其引用的对象在堆区

## Finally中的return语句

如果finally块中有return 语句的话，它将覆盖掉函数中其他return语句。

## java数据库连接库JDBC用到哪种设计模式

桥接模式

## ServletContext

getParameter() 是获取POST/GET传递的参数值；

getInitParameter获取Tomcat的server.xml中设置Context的初始化参数

getAttribute()是获取对象容器中的数据值；

getRequestDispatcher是请求转发。

## final、finally、finalize

### final

final修饰变量，变量的引用（也就是指向的地址）不可变，但是引用的内容可以变（地址中的内容可变）

```java
final String a = "b";
a = "b"//编译通过
a = new String("c");//出错 
```

### finally

表示总执行。

### finalize

这个方法一个对象只能执行一次，只能在第一次进入被回收的队列，而且对象属于的类重写了finalize方法才会被执行。第二次进入回收队列的时候，不会再执行其finalize方法，而是直接被二次标记，在下一次GC的时候被GC。

![](img/finalize方法示意图.png)

## **ServerSocket** (int port)

创建一个serversocket 绑定在特定的端口

## **Socket**(InetAddress address, int port)]

创建一个socket流，连接到特定的端口和ip地址

## 依赖注入的动机就是减少组件之间的耦合度，使开发更为简洁

## 内部类

### **1. 静态内部类：**

  **1. 静态内部类本身==可以访问外部的静态资源，包括静态私有资源。但是不能访问非静态资源，可以不依赖外部类实例而实例化==。**

### **2. 成员内部类：**

  **1. 成员内部类本身可以访问外部的所有资源，但是==自身不能定义静态资源，因为其实例化本身就还依赖着外部类==。**

### **3. 局部内部类：**

  **1. 局部内部类就像一个局部方法，不能被访问修饰符修饰，也不能被static修饰。**

  **2. 局部内部类只能访问所在代码块或者方法中被定义为final的局部变量。**

### **4. 匿名内部类：**

  **1. 没有类名的内部类，不能使用class，extends和implements，没有构造方法。**

  **2. 多用于GUI中的事件处理。**

  **3. 不能定义静态资源**

  **4. 只能创建一个匿名内部类实例。**

  **5. 一个匿名内部类一定是在new后面的，这个匿名类必须继承一个父类或者实现一个接口。**

  **6. 匿名内部类是局部内部类的特殊形式，所以局部内部类的所有限制对匿名内部类也有效。**

# 2022.1.14

## abstract

abstract修饰的类需要继承，而final修饰的类不可被继承。

## 抽象类使用原则

抽象方法必须为public或者protected（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public；

抽象类不能直接实例化，需要依靠子类采用向上转型的方式处理；

抽象类必须有子类，使用extend是继承，一个子类只能继承一个抽象类；

子类（如果不是抽象类）则必须复写抽象类之中的全部抽象方法（如果子类没有实现父类的抽象方法，则必须将子类也定义为abstract类）

## 线程sleep，yield，wait，join

- sleep会使当前线程睡眠指定时间，不释放锁
- yield会使当前线程重回到可执行状态，等待cpu的调度，不释放锁
- wait会使当前线程回到线程池中等待，释放锁，当被其他线程使用notify,notiftAll唤醒时进入可执行状态
- 当前线程调用某线程.join()时会使当前线程等待某线程执行完毕再结束，底层调用了wait，释放锁

## 抽象类可以有构造函数，只是不能实例化，

## java系统3种加载器

启动类加载器（Bootstrap ClassLoader），扩展类加载器（Extension ClassLoader），应用程序类加载器（Application ClassLoader）

## java虚拟机中的唯一性

对任意一个类，都需要由加载器和这个类本身一同确立其在java虚拟机中的唯一性，每一个类加载器，都拥有一个独立的类名称空间。接口类是一种特殊类，因此对于同一接口不同的类装载器装载所获的类是不相同的。

## 类只需要加载一次就行，因此要保证加载过程线程安全，防止类加载多次

## 类加载器的双亲委派模型

Java程序的类加载器采用双亲委派模型，实现双亲委派的代码集中在java.lang.ClassLoader的loadClass()方法中，此方法实现的大致逻辑是：先检查是否已经被加载，若没有加载则调用父类加载器的loadClass()方法，若父类加载器为空则默认使用启动类加载器作为父类加载器。如果父类加载失败，抛出ClassNotFoundException异常

## 双亲委派模型工作过程

如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，每一层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的启动类加载器中，只有当父加载器反馈自己无法完成这个加载请求时，子加载器才会尝试自己去加载。

## 应用程序类加载器（Application ClassLoader）

负责加载用户类路径（ClassPath）上所指定的类库，不是所有的ClassLoader都加载此路径

## java.util.Collection与java.util.Collections

java.util.Collection 是一个集合接口。它提供了对集合对象进行基本操作的通用接口方法。Collection接口在Java 类库中有很多具体的实现。Collection接口的意义是为各种具体的集合提供了最大化的统一操作方式。

java.util.Collections 是一个包装类。它包含有各种有关集合操作的静态多态方法。此类不能实例化，就像一个工具类，服务于Java的Collection框架。

# 2022.1.15

## 匿名内部类

匿名内部类的创建格式为 

```java
new 父类构造器（参数列表）|实现接口（）{
	//匿名内部类的类体实现
}
```

使用匿名内部类时，必须继承一个类或实现一个接口。

匿名内部类由于没有名字，因此不能定义构造函数

匿名内部类不能含有静态变量和静态方法

## Java有5种方式来创建对象： 

1、使用 new 关键字（最常用）： ObjectName obj = new ObjectName(); 

2、使用反射的Class类的newInstance()方法： ObjectName obj = ObjectName.class.newInstance();

 3、使用反射的Constructor类的newInstance()方法： ObjectName obj = ObjectName.class.getConstructor.newInstance(); 

4、使用对象克隆clone()方法： ObjectName obj = obj.clone(); 

5、使用反序列化（ObjectInputStream）的readObject()方法： try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILE_NAME))) { ObjectName obj = ois.readObject(); }

# 2022.1.16

## 线程的start和run

调用start则先执行主线程的，后执行子线程的，

调用run相当于函数调用。顺序执行。

## **[Java](http://lib.csdn.net/base/javase)中的volatile关键字的功能**

volatile是java中的一个类型修饰符。它是被设计用来修饰被不同线程访问和修改的变量。如果不加入volatile，基本上会导致这样的结果：要么无法编写多线程程序，要么编译器 失去大量优化的机会。

1，可见性

  可见性指的是在一个线程中对该变量的修改会马上由工作内存（Work Memory）写回主内存（Main Memory），所以会马上反应在其它线程的读取操作中。顺便一提，工作内存和主内存可以近似理解为实际电脑中的高速缓存和主存，工作内存是线程独享的，主存是线程共享的。

2，禁止指令重排序优化

  禁止指令重排序优化。大家知道我们写的代码（尤其是多线程代码），由于编译器优化，在实际执行的时候可能与我们编写的顺序不同。编译器只保证程序执行结果与源代码相同，却不保证实际指令的顺序与源代码相同。这在单线程看起来没什么问题，然而一旦引入多线程，这种乱序就可能导致严重问题。volatile关键字就可以从语义上解决这个问题。

  注意，禁止指令重排优化这条语义直到jdk1.5以后才能正确工作。此前的JDK中即使将变量声明为volatile也无法完全避免重排序所导致的问题。所以，在jdk1.5版本前，双重检查锁形式的单例模式是无法保证线程安全的。因此，下面的单例模式的代码，在JDK1.5之前是不能保证线程安全的。

## CopyOnWrite适用于读多写少的并发场景

## ConcurrentHashMap，读操作不需要加锁，写操作需要加锁

## yield的作用

yield()让当前正在运行的线程回到可运行状态，以允许具有相同优先级的其他线程获得运行的机会。因此，使用yield()的目的是让具有相同优先级的线程之间能够适当的轮换执行。但是，实际中无法保证yield()达到让步的目的，因为，让步的线程可能被线程调度程序再次选中。

# 2022.1.17

## 常见ASCII码

- 空格ASCII码值为32
- 数字0到9的ASCII值分别为48到57
- 大写字母A到Z的ASCII码值为65到90
- 小写字母a到z的ASCII码值为97到122。

## 常见压缩命令参数

- -s 还原文件的顺序和备份文件内的存放顺序相同。
- -t 列出备份文件的内容。
- -v 显示指令执行过程。
- -f 指定压缩文件
- -x 从备份文件中还原文件

## Math.round()  四舍五入，返回int类型

## Math.ceil()  表示向上取整，返回double类型   （ceil---天花板）

## Math.ceil()  表示向上取整，返回double类型   （ceil---天花板）

## final

1.final修饰变量，则等同于常量

2.final修饰方法中的参数，称为最终参数。

3.final修饰类，则类不能被继承

4.final修饰方法，则方法不能被重写。

5.final 不能修饰抽象类

6.final修饰的方法可以被重载 但不能被重写

# 2022.1.18

## Object类中方法及说明

```apl
registerNactives() //私有方法
getClass() //返回此Object的运行类
hashCode()//用于获取对象的哈希值。
equals(Object obj) //用于确认两个对象是否相同
clone() //创建并返回对象的一个副本
toString() //返回该对象的字符串表示。
notify() //唤醒在此对象监视器上等待的单个线程。
notifyAll() //唤醒在此对象监视器上等待的所有线程，
wait(long timeout)//在其他线程调用此对象的notify()方法或notifyAll()方法，或者超过都指定的时间量前，导致当线程等待
wait(long timeout,int nanos)//在其他线程调用此对象的notify()方法或notifyAll()方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量前，导致当前线程等待
wait() //用于让当前线程失去操作权限，当前线程进入等待序列
finalize() //当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。               

```

## 排序算法时间复杂度与空间复杂度

![](img/排序算法时间复杂度空间复杂度.png)

|，||，&，&&

**|| 与 && 都是短路功能：**

**前者，表达式一为真，表达式二不执行。**

**后者，表达式一位假，表达式二不执行。**

**| 与 &不具备短路功能**

## Java线程的类型

### 用户线程

​	通过Thread.setDaemon(false)设置为用户线程

### 守护线程

​	通过Thead.setDaemon(true),设置为守护线程，如果不设置，默认用户线程；

守护线程是服务用户线程的线程，在它启动之前必须先set。

## 内存回收

​	内存回收程序负责释放无用内存。

​	JVM一旦启动，就会创建一个守护线程来监测是否需要有对象内存被释放。

​	无法直接释放

​	不可以指定时间，System.gc(),只是提醒jvm可以进行一次Full GC，但是什么时候真正执行，还是不知道的。

## java异常

**注意：异常和错误的区别：异常能被程序本身可以处理，错误是无法处理**

![](img/java异常的分类和类结构图.png)

总体上我们根据Javac对异常的处理要求，将异常类分为2类。

### **非检查异常**

**非检查异常**（unckecked exception）：Error 和 RuntimeException 以及他们的子类。==javac在编译时，不会提示和发现这样的异常==，不要求在程序处理这些异常。所以如果愿意，我们可以编写代码处理（使用try...catch...finally）这样的异常，也可以不处理。对于这些异常，我们应该修正代码，而不是去通过异常处理器处理 。这样的异常发生的原因多半是代码写的有问题。如除0错误ArithmeticException，错误的强制类型转换错误ClassCastException，数组索引越界ArrayIndexOutOfBoundsException，使用了空对象NullPointerException等等。

### 检查异常

**检查异常**（checked exception）：除了Error 和 RuntimeException的其它异常。==javac强制要求程序员为这样的异常做预备处理工作==（使用try...catch...finally或者throws）。在方法中要么用try-catch语句捕获它并处理，要么用throws子句声明抛出它，否则编译不会通过。这样的异常一般是由程序的运行环境导致的。因为程序可能被运行在各种未知的环境下，而程序员无法干预用户如何使用他编写的程序，于是程序员就应该为这样的异常时刻准备着。如SQLException , IOException,ClassNotFoundException 等。

## OSI参考模型

![](img/OSI参考模型.png)

# 2022.1.19

## hashcode和equals约定关系如下

1. 如果两个对象相等，那么他们一定有相同的哈希值
2. 如果两个对象的哈希值相等，那么这两个对象有可能相等也有可能不相等，

## 加载驱动的方法

```java
Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
DriverManager.registerDriver(new com.mysql.jdbc.Driver());
System.setProperty("jdbc.drivers", "com.mysql.jdbc.Driver");
```

## Java中的char

java中的char是Unicode编码，Unicode编码占两个字节，就是16位，足够存储一个汉字。

## Properties实现了Map接口，是线程安全的

## Java关键字

​	true、false、null都不是关键字。

​	goto、const 是保留的关键字。

```apl
abstract                continue           for            new        
switch                  default            if             package     
synchronized            do                 goto           private     
this                    break              double         implements   
protected               throw              byte           else       
import                  public             throws         case       
enum                    instanceof         return         transient  
catch                   extends            int            short       
try                     char               final          interface    
static                  void               class          finally   
long                    strictfp           volatile       const      
float                   native             super          while
boolean                 assert 
```

## 变量的引用

常量池：未经 new 的常量

堆区：成员变量的引用，new 出来的变量

栈区：局部变量的引用

成员变量的引用在堆区，是因为成员变量的所属对象在堆区，所以它也在堆区。

局部变量的引用在栈区，是因为局部变量不属于某一个对象，在被调用时才被加载，所以在栈区。

## String，StringBuffer，StringBuilder的区别

### 可变与不可变

​	String 类中使用字符数组保存字符串，如下就是，因为有final修饰符，所以可以知道String对象是不可变的。String为不可变对象，一旦被创建，就不能修改它的值，对于已经存在可的String对象的修改都是重新创建一个新的对象，然后把新的值保存进去。

```
 private final char value[];
```

​	StringBuilder与StringBuffer都继承自AbstractStringBuilder类，在AbstractStringBuilder中也是使用字符数组保存字符串，如下就是，可知这两种对象都是可变的。

```java
char[] value
```

StringBuffer是一个可变对象，当对他进行修改的时候不会像String那样重新建立对象，它只能通过构造函数来建立。如：StringBuffer sb = new StringBuffer();不能通过赋值符号对他进行赋值。对象被建立以后,在内存中就会分配内存空间,并初始保存一个null.向StringBuffer中赋值的时候可以通过它的append方法.sb.append("hello");

### 线程安全

​	String中的对象是不可变的，也就可以理解为常量，显然线程安全。

​	AbstractStringBuilder是StringBuilder与StringBuffer的公共父类，定义了一些字符串的基本操作，如expandCapacity、append、insert、indexOf等公共方法。

​	**StringBuffer对方法加了同步锁或者对调用的方法加了同步锁，所以是** **线程安全的** **。看如下源码：**

```java
public synchronized StringBuffer reverse() {
	 super.reverse();
     return this ;
}

public int indexOf(String str) {
    return indexOf(str, 0);//存在 public synchronized int indexOf(String str, int fromIndex) 方法
}
```

**StringBuilder并没有对方法进行加同步锁，所以是** **非线程安全的** **。**

**StringBuilder与StringBuffer共同点**

**StringBuilder与StringBuffer有公共父类AbstractStringBuilder(** **抽象类** **)。**

**抽象类与接口的其中一个区别是：抽象类中可以定义一些子类的公共方法，子类只需要增加新的功能，不需要重复写已经存在的方法；而接口中只是对方法的申明和常量的定义。**

**StringBuilder、StringBuffer的方法都会调用AbstractStringBuilder中的公共方法，如super.append(...)。只是StringBuffer会在方法上加synchronized关键字，进行同步。**

### 效率

最后，如果程序不是多线程的，那么使用StringBuilder效率高于StringBuffer。 

效率比较String < StringBuffer < StringBuilder，但是在String S1 =“This is only a”+“simple”+“test”时，String效率最高

# 2022.1.20

## Vector&ArrayList的主要区别

1. 同步性Vector是线程安全的，也就是说是同步的，而ArrayList是线程不安全的，不是同步的

2. 数据增长：当需要增长时，Vector默认增长为原来一倍，而ArrayList却是原来的50%，这样ArrayList就有利于节约内存空间。

   如果涉及到堆栈、队列等操作，应该考虑用Vector，如果需要快速随机访问元素，应该使用ArrayList

## HashMap

1. HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体，HashMap的底层结构是一个数组，数组中的每一项是一条链表。
2. HashMap的实例有俩个参数影响其性能：‘初始容量’和‘装填因子’
3. HashMap实现不同步，线程不安全。HashTable线程安全
4. HashMap中的key-value都是存储在Entry中的。
5. HashMap可以存null键和null值，不保证元素的顺序恒久不变，它的底层使用时数组和链表，通过hashCode()方法和equals方法保证键的唯一性，
6. 。HashMap是采用拉链法解决哈希冲突的。

## 解决冲突的方法

### 分类

​	解决冲突主要有三种方法：定址法，拉链法，再散列法

### 链表法

​	链表法是将相同hash值的对象组成一个链表放在hash值对应的槽位

### 定址法

​	当冲突发生时，使用某种探查技术在散列表中形成一个探查序列。沿此序列逐个单元地查找，直到找到给定的关键字，或者碰到一个开发的地址（即该地址单元为空）为止（若要插入，在探查到开放的地址，则可将待插入的新结点存入该地址单元）。

### 拉链法

​	拉链法解决冲突的做法是：将所有关键字为同义词的结点链接到在同一个单链表中。若选定的散列表长度为m，则可将散列定义为一个由m个头指针组成的指针数组T[0..m-1]。凡是散列地址为i的结点，均插入到以T[i]为头指针的单链表中。T中各分量的初值均应为空指针。在拉链法中，装填因子在拉链法中，装填因子α可以大于1，但一般均取α≤1。拉链法适合未规定元素的大小

## Hashtable和HashMap的区别

​	a)  继承不同。  public class Hashtable extends Dictionary implements Map public class HashMap extends  AbstractMap implements Map

​	 b) Hashtable中的方法是同步的，而HashMap中的方法在缺省情况下是非同步的。在多线程并发的环境下，可以直接使用Hashtable，但是要使用HashMap的话就要自己增加同步处理了。

 	c) Hashtable 中， key 和 value 都不允许出现 null 值。 在 HashMap 中， null 可以作为键，这样的键只有一个；可以有一个或多个键所对应的值为 null 。当 get() 方法返回 null 值时，即可以表示 HashMap 中没有该键，也可以表示该键所对应的值为 null 。因此，在 HashMap 中不能由 get() 方法来判断 HashMap 中是否存在某个键， 而应该用 containsKey() 方法来判断。 

​	d) 两个遍历方式的内部实现上不同。Hashtable、HashMap都使用了Iterator。而由于历史原因，Hashtable还使用了Enumeration的方式 。

 	e) 哈希值的使用不同，HashTable直接使用对象的hashCode。而HashMap重新计算hash值。

​	 f) Hashtable和HashMap它们两个内部实现方式的数组的初始大小和扩容的方式。HashTable中hash数组默认大小是11，增加的方式是old*2+1。HashMap中hash数组的默认大小是16，而且一定是2的指数。  注：  HashSet子类依靠hashCode()和equal()方法来区分重复元素。    HashSet内部使用Map保存数据，即将HashSet的数据作为Map的key值保存，这也是HashSet中元素不能重复的原因。而Map中保存key值的,会去判断当前Map中是否含有该Key对象，内部是先通过key的hashCode,确定有相同的hashCode之后，再通过equals方法判断是否相同。

## 创建子类的对象时，会先执行父类的构造函数。

## 子类构造器

​	子类构造器的默认第一行就是super()，默认调用直接父类的无参构造。这也就是一旦一个子类的直接父类没有无参的构造的情况下，必须在自己构造器的第一行显式的指明调用父类或者自己的哪一个构造器

## Ant和Maven

​	Ant和Maven都是基于Java的构建（build）工具。

### Ant特点

- 没有一个约定的目录结构，
- 必须明确让ant做什么，什么时候做，然后编译，打包
- 没有生命周期，必须定义目标及其实现的任务序列
- 没有集成依赖管理

### Maven特点

- 拥有约定，知道你的代码在哪里。放到哪里去
- 拥有一个生命周期，例如执行 mvn install 就可以自动执行编译，测试，打包等构建过程
- 只需要定义一个pom.xml,然后把源码放到默认的目录，Maven帮你处理其他事情
- 拥有依赖管理，仓库管理

## 抽象类

- 抽象类可以有构造方法，接口没有构造方法
- 抽象类可以有普通成员变量，接口没有普通成员变量
- 抽象类和接口都可有静态成员变量，抽象类中静态成员变量访问类型任意，接口只能public static final（默认）
- 抽象类可以没有抽象方法，抽象类可以有普通方法，接口中都是抽象方法。
- 抽象类可以有静态方法，接口不能有静态方法
- 抽象类中的方法可以是public，protected；接口方法只有public。

## HttpServletResponse设置响应头的数据

```java
public void setDateHeader(String name,long date);
public void addDateHeader(String name,long date);
public void setHeader(String name,String value);
public void addHeader(String name,String value);
public void setIntHeader(String name,int value);
public void addIntHeader(String name,int value);
```

## 值传递和引用传递

- **值传递**是将变量的一个副本传递到方法中，方法中如何操作该变量副本，都不会改变原变量的值。
- **引用传递**是将变量的内存地址传递给方法，方法操作变量时会找到保存在该地址的变量，对其进行操作。会对原变量造成影响。

# 2022.1.21

## 四大修饰符

### \- private(私有的) 

​	private可以修饰成员变量，成员方法，构造方法，不能修饰类(此刻指的是外部类，内部类不加以考虑)。被private修饰的成员只能在其修饰的本类中访问，在其他类中不能调用，但是被private修饰的成员可以通过set和get方法向外界提供访问方式 

### \- default(默认的) 

​	defalut即不写任何关键字，它可以修饰类，成员变量，成员方法，构造方法。被默认权限修饰后，其只能被本类以及同包下的其他类访问。 

### \- protected(受保护的) 

​	protected可以修饰成员变量，成员方法，构造方法，但不能修饰类(此处指的是外部类，内部类不加以考虑)。被protected修饰后，只能被同包下的其他类访问。如果不同包下的类要访问被protected修饰的成员，这个类必须是其子类。 

### \- public(公共的) 

​	public是权限最大的修饰符，他可以修饰类，成员变量，成员方法，构造方法。被public修饰后，可以再任何一个类中，不管同不同包，任意使用。 

|          | public | protected | default | private |
| -------- | ------ | --------- | ------- | ------- |
| 同一个类 | √      | √         | √       | √       |
| 同一个包 | √      | √         | √       |         |
| 子类     | √      | √         |         |         |
| 不同包   | √      |           |         |         |



## 枚举（enum）

​	枚举（enum）类型是java 5新增的特性，它是一种新的类型，允许用常量来表示特定的数据片断，而且全部都以类型安全的形式来表示，是特殊的类，可以拥有成员变量和方法。

## hashMap，hashTable，concurrentHashMap

​	hashMap在单线程中使用大大提高效率，在多线程的情况下使用hashTable来确保安全。hashTable中使用synchronized关键字来实现安全机制，但是synchronized是对整张hash表进行锁定即让线程独享整张hash表，在安全同时造成了浪费。concurrentHashMap采用分段加锁的机制来确保安全。（jdk1.8后。concurrentHashMap取消了segment分段锁，采用cas和synchronized来保证并发安全）



## java异常处理

#### throw用于抛出异常

#### throws 关键字可以在方法上声明该方法要抛出的异常、然后在方法内部通过throw抛出异常对象。

#### try是用于检测被包住的语句块是否出现异常，如果有异常，则抛出异常，并执行catch语句。

#### cacth用于捕获从try中抛出的异常并作出处理。

#### finally语句块是不管有没有出现异常都要执行的内容。

## 单例模式

### 定义

​	确保一个类只有一个实例，并提供该实例的全局访问点。这样做的好处是：有些实例，全局只需要一个就够了，使用单例模式就可以避免一个全局使用的类，频繁的创建与销毁，耗费系统资源。

## 设计要素

- 一个私有构造函数（确保只能单例类自己创建实例）
- 一个私有静态变量（确保只有一个实例）
- 一个公有静态函数（给使用者提供调用方法）

简单来说就是，单例类的构造方不让其他人修改和使用；并且单例类自己只创建一个实例，这个实例，其他人也无法修改和直接使用；然后单例类提供一个调用方法，想用这个实例，只能调用。这样就确保了全局只创建了一个实例。

### 单例模型6种实现及各实现的优缺点

#### 一、懒汉式（线程不安全）

实现

```java
public class Singleton {
    private static Singleton uniqueInstance;
    private Singleton() {}
    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}
```

**说明**：先不创建实例，当第一次被调用时，在创建实例，所以被称为懒汉式。

**优点**：延迟了实例化，如果不需要使用该类，就不会被实例化，节约了系统资源。

**缺点**：线程不安全，多线程环境下，如果多个线程同时进入了 if (uniqueInstance == null) ，若此时还未实例化，也就是uniqueInstance == null，那么就会有多个线程执行 uniqueInstance = new Singleton(); ，就会实例化多个实例；

#### 二、饿汉式（线程安全）

实现：

```java
public class Singleton{
	private static Singleton uniqueInstance = new Singleton();
	private Singleton(){
	}
	public static Singleton getUniqueInstance(){
		return uniqueInstance;
	}
}
```

**说明**：先不管需不需要使用这个实例，直接先实例化好实例（饿死鬼一样，所以称为饿汉式），然后当需要使用的时候，直接调方法就可以使用了

**优点**：提起实例化好了一个实例，避免了线程不安全问题的出现，

**缺点**：直接实例化了实例，不再延迟实例化；若系统没有使用这个实例，或者系统运行很久之后才需要使用这个实例，都会使操作系统的资源浪费。

#### （三）懒汉式（线程安全）

**实现：**

```java
public class Singleton {
    private static Singleton uniqueInstance;

    private static singleton() {
    }

    private static synchronized Singleton getUinqueInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }

}
```

**说明：** 实现和 线程不安全的懒汉式 几乎一样，唯一不同的点是，在get方法上 加了一把 锁。如此一来，多个线程访问，每次只有拿到锁的的线程能够进入该方法，避免了多线程不安全问题的出现。

**优点：** 延迟实例化，节约了资源，并且是线程安全的。

**缺点：** 虽然解决了线程安全问题，但是性能降低了。因为，即使实例已经实例化了，既后续不会再出现线程安全问题了，但是锁还在，每次还是只能拿到锁的线程进入该方***使线程阻塞，等待时间过长。

#### （四）双重检查锁实现（线程安全）

**实现：**

```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }  
}
```

**说明:** 双重检查数相当于是改进了 线程安全的懒汉式。线程安全的懒汉式 的缺点是性能降低了，造成的原因是因为即使实例已经实例化，依然每次都会有锁。而现在，我们将锁的位置变了，并且多加了一个检查。 也就是，先判断实例是否已经存在，若已经存在了，则不会执行判断方法内的有锁方法了。 而如果，还没有实例化的时候，多个线程进去了，也没有事，因为里面的方法有锁，只会让一个线程进入最内层方法并实例化实例。如此一来，最多最多，也就是第一次实例化的时候，会有线程阻塞的情况，后续便不会再有线程阻塞的问题。

**为什么使用 volatile 关键字修饰了 uniqueInstance 实例变量 ？**

uniqueInstance = new Singleton(); 这段代码执行时分为三步：

1. 为 uniqueInstance 分配内存空间
2. 初始化 uniqueInstance
3. 将 uniqueInstance 指向分配的内存地址

正常的执行顺序当然是 1>2>3 ，但是由于 JVM 具有指令重排的特性，执行顺序有可能变成 1>3>2。
单线程环境时，指令重排并没有什么问题；多线程环境时，会导致有些线程可能会获取到还没初始化的实例。
例如：线程A 只执行了 1 和 3 ，此时线程B来调用 getUniqueInstance()，发现 uniqueInstance 不为空，便获取 uniqueInstance 实例，但是其实此时的 uniqueInstance 还没有初始化。

解决办法就是加一个 volatile 关键字修饰 uniqueInstance ，volatile 会禁止 JVM 的指令重排，就可以保证多线程环境下的安全运行。

**优点：** 延迟实例化，节约了资源；线程安全；并且相对于 线程安全的懒汉式，性能提高了。

**缺点：** volatile 关键字，对性能也有一些影响。

#### （五）静态内部类实现（线程安全）

**实现：**

```java
public class Singleton {

    private Singleton() {
    }

    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getUniqueInstance() {
        return SingletonHolder.INSTANCE;
    }

}
```

**说明：** 首先，当外部类 Singleton 被加载时，静态内部类 SingletonHolder 并没有被加载进内存。当调用 getUniqueInstance() 方法时，会运行 return SingletonHolder.INSTANCE; ，触发了 SingletonHolder.INSTANCE ，此时静态内部类 SingletonHolder 才会被加载进内存，并且初始化 INSTANCE 实例，而且 JVM 会确保 INSTANCE 只被实例化一次。

**优点：** 延迟实例化，节约了资源；且线程安全；性能也提高了。

#### （六）枚举类实现（线程安全）

**实现：**

```java
public enum Singleton {

    INSTANCE;

    //添加自己需要的操作
    public void doSomeThing() {

    }

}
```

**说明：** 默认枚举实例的创建就是线程安全的，且在任何情况下都是单例。

**优点：** 写法简单，线程安全，天然防止反射和反序列化调用。

- **防止反序列化**
  **序列化：**把java对象转换为字节序列的过程；
  **反序列化：** 通过这些字节序列在内存中新建java对象的过程；
  **说明：** 反序列化 将一个单例实例对象写到磁盘再读回来，从而获得了一个新的实例。
  我们要防止反序列化，避免得到多个实例。
  **枚举类天然防止反序列化。**
  其他单例模式 可以通过 重写 readResolve() 方法，从而防止反序列化，使实例唯一重写 readResolve() :

```
private` `Object readResolve() ``throws` `ObjectStreamException{``    ``return` `singleton;``}
```

#### 四、单例模式的应用场景

**应用场景举例：**

- 网站计数器。
- 应用程序的日志应用。
- Web项目中的配置对象的读取。
- 数据库连接池。
- 多线程池。
- ......

**使用场景总结：**

- **频繁实例化然后又销毁的对象**，使用单例模式可以提高性能。
- **经常使用的对象，但实例化时耗费时间或者资源多**，如数据库连接池，使用单例模式，可以提高性能，降低资源损坏。
- **使用线程池之类的控制资源时**，使用单例模式，可以方便资源之间的通信。

# 2022.1.22

## statement与PreparedStatement

statement每次执行SQL语句，数据库都要执行SQL语句的编译，最好用于仅一次查询并返回结果的情形，效率高于PreparedStatement

PrepareStatement是预编译的，使用PreparedStatement有几个好处

- 安全性好，有效防止Sql注入等问题
- 对于多次重复执行的语句，使用PreparedStatment效率会更高一点，并且在这种情况下也比较适合使用batch
- 代码的可读性和可维护性。

CallableStatement接口扩展PrepareStatement，用来调用存储过程,它提供了对输出和输入/输出参数的支持。CallableStatement 接口还具有对 PreparedStatement 接口提供的输入参数的支持。垃圾回收在jvm中优先级相当相当低。



## 垃圾收集器（GC）程序开发者只能推荐JVM进行回收，但何时回收，回收哪些，程序员不能控制。

## 进入DEAD的线程，它还可以恢复，GC不会回收

## 垃圾回收在jvm中优先级相当相当低



## ThreadLocal

ThreadLocal用于线程之间的数据隔离，ThreadLocal类创建一个线程本地变量。在Thread中有一个成员变量ThreadLocals，该变量的类型是ThreadLocalMap，也就是一个Map。它的键是ThreadLocal，值为就是变量的副本。通过ThreadLocal的get()方法可以获取该线程变量的本地副本，在get方法之前要先set，否则就要重写initalValue()方法。

对于多线程资源共享的问题，同步机制采用了“以时间换空间”的方式，而ThreadLocal采用了“以空间换时间”的方式。前者仅提供一份变量，让不同的线程排队访问，而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响

## ThreadLocal的使用场景

​    数据库连接：在多线程中，如果使用懒汉式的单例模式创建Connection对象，由于该对象是共享的，那么必须要使用同步方法保证线程安全，这样当一个线程在连接数据库时，那么另外一个线程只能等待。这样就造成性能降低。如果改为哪里要连接数据库就来进行连接，那么就会频繁的对数据库进行连接，性能还是不高。这时使用ThreadLocal就可以既可以保证线程安全又可以让性能不会太低。但是ThreadLocal的缺点时占用了较多的空间。



## AOP 和 OOP的区别：

1. **面向方面编程** **AOP** 偏重业务处理过程的某个步骤或阶段，强调降低模块之间的耦合度，使代码拥有更好的移植性。

2. **面向对象编程** **(oop)** 则是对业务分析中抽取的实体进行方法和属性的封装。

**也可以说** **AOP** **是面向业务中的动词领域，** **OOP** **面向名词领域。**

# 2022.1.23

![](img/内部类.png)

## jvm参数

-Xmx10240m：代表最大堆

 -Xms10240m：代表最小堆

 -Xmn5120m：代表新生代

 -XXSurvivorRatio=3：代表Eden:Survivor = 3  根据Generation-Collection算法(目前大部分JVM采用的算法)，一般根据对象的生存周期将堆内存分为若干不同的区域，一般情况将新生代分为Eden ，两块Survivor；   计算Survivor大小， Eden:Survivor = 3，总大小为5120,3x+x+x=5120  x=1024

新生代大部分要回收，采用Copying算法，快！

老年代 大部分不需要回收，采用Mark-Compact算法

# 2022.1.24

**如果类没有构造发方法，JVM会生成一个默认构造方法，如果定义了任意类型的构造方法，编译器都不会自动生成构造方法**

****

**~n=-n-1** 



## Mysql组合索引（复合索引）最左优先原则

​	最左优先就是说组合索引的第一个字段必须出现在查询组句中，这个索引才会被用到。只要组合索引最左边第一个出现在where中，那么不管后面的字段出现与否或者出现顺序如何，Mysql引擎都会自动调用索引来优化查询效率。



## 正则表达式的规则

1. 任意一个字符表示匹配任意对应的字符，如a匹配a，7匹配7，-匹配-。
2. []代表匹配中括号中其中任一字符，如[abc]匹配a或b或c。
3. -在中括号里面和外面代表含义不同，如在外时，就匹配-，如果在中括号内[a-b]表示匹配26个小写字母中的任一个；[a-zA-Z]匹配大小写共52个字母中任一个；[0-9]匹配十个数字中任一个。
4. ^在中括号里面和外面含义不同，如在外时，就表示开头，如 ^7[0-9]表示匹配开头是7的，且第二位是任一数字的字符串；如果在中括号里面，表示除了这个字符意外的任意字符（包括数字，特殊字符），如[ ^abc]表示匹配出去abc之外的其他任一字符，
5. .表示匹配任意的字符。
6. \d表示数字
7. \D表示非数字
8. \s表示由空字符组成，[ \t\n\r\x\f]。
9. \S表示由非空字符组成，[ ^ \s]。
10. \w表示字母、数字、下划线，[a-zA-Z0-9_]。
11.  \W表示不是由字母、数字、下划线组成。
12. ?: 表示出现0次或1次。
13. +表示出现1次或多次。
14.  *表示出现0次、1次或多次。
15.  {n}表示出现n次。
16. {n,m}表示出现n~m次。
17.  {n,}表示出现n次或n次以上。
18.  XY表示X后面跟着Y，这里X和Y分别是正则表达式的一部分。
19.  X|Y表示X或Y，比如"food|f"匹配的是foo（d或f），而"(food)|f"匹配的是food或f。
20.  (X)子表达式，将X看做是一个整体



![](img/Servlet生命周期.png)

## 方法重载

- 方法名相同	
- 方法的参数类型，参数个数不一样
- 方法的返回类型可以不相同
- 方法的修饰符可以不相同。



## 类加载过程

### 概述

​	类从被加载到虚拟机内存中开始，到卸载出内存为止，它的整个生命周期包括：加载（Loading）、验证（Verification）

，准备（Preparation） 、解析（Resolution）、初始化（Initialzation）、使用（Using）和卸载（Unloading ）7个阶段。

其中准备、验证、解析3个部分统称为连接（Linking）。如图所示。

​	![](img/生命周期.png)

加载、验证、准备、初始化和卸载这5个阶段的顺序是确定的，类的加载过程必须按照这种顺序按部就班地开始，而解析阶段则不一定；它在某些情况下可以在初始化阶段之后再开始，这是为了支持Java语言的运行时绑定（也称为动态绑定或晚期绑定）。

### 加载

​	在加载阶段（可以参考java.lang.ClassLoader的localClass() 方法），虚拟机需要完成以下3件事件

1. 通过一个类的全限定名来获取定义此类的二进制字节流；
2. 将这个字节流所表示的静态存储结构转化为方法区的运行时数据结构
3. 在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口；加载阶段和连接阶段（Linked）

的部分内容（如一部分字节码文件格式验证动作）是交叉进行的，加载阶段尚未完成，连接阶段可能已经开始，但这些夹在加载阶段之中进行的动作，仍然属于连接阶段的内容，这两个阶段的开始时间仍然保持着固定的先后顺序。

## 验证

​	验证是连接阶段的第一步，这一阶段的目的是为了确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。

​	验证阶段大致会完成4个阶段的检验动作：

1. 文件格式验证：验证字节流是否符合Class文件格式的规范；例如：是否以魔术0xCAFEBABE开头、主次版本号是否在当前虚拟机的处理范围之内、常量池中的常量是否有不被支持的类型。
2. 元数据验证：对字节码描述的信息进行语义分析（注意：对比javac编译阶段的语义分析），以保证其描述的信息符合Java语言规范的要求；例如：这个类是否有父类，除了java.lang.Object之外。
3. 字节码验证：通过数据流和控制流分析，确定程序语义是合法的、符合逻辑的。
4. 符号引用验证：确保解析动作能正确执行。

验证阶段是非常重要的，但不是必须的，它对程序运行期没有影响，如果所引用的类经过反复验证，那么可以考虑采用-Xverifynone参数来关闭大部分的类验证措施，以缩短虚拟机类加载的时间。

### **准备**

​	准备阶段是正式为类变量分配内存并设置类变量初始值的阶段，这些变量所使用的内存都将在方法区中进行分配。这时候进行内存分配的仅包括类变量（被static修饰的变量），而不包括实例变量，实例变量将会在对象实例化时随着对象一起分配在堆中。其次，这里所说的初始值“通常情况”下是数据类型的零值，假设一个类变量的定义为：

| 1    | publicstaticintvalue=123; |
| ---- | ------------------------- |
|      |                           |

​	那变量value在准备阶段过后的初始值为0而不是123.因为这时候尚未开始执行任何java方法，而把value赋值为123的putstatic指令是程序被编译后，存放于类构造器()方法之中，所以把value赋值为123的动作将在初始化阶段才会执行。
至于“特殊情况”是指：public static final int value=123，即当类字段的字段属性是ConstantValue时，会在准备阶段初始化为指定的值，所以标注为final之后，value的值在准备阶段初始化为123而非0.

### **解析**

​	解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程。解析动作主要针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符7类符号引用进行。

### **初始化**

​	类初始化阶段是类加载过程的最后一步，到了初始化阶段，才真正开始执行类中定义的java程序代码。在准备阶段，变量已经付过一次系统要求的初始值，而在初始化阶段，则根据程序猿通过程序制定的主管计划去初始化类变量和其他资源，或者说：初始化阶段是执行类构造器<clinit>()方法的过程.

<clinit>()方法是由编译器自动收集类中的所有类变量的赋值动作和静态语句块static{}中的语句合并产生的，编译器收集的顺序是由语句在源文件中出现的顺序所决定的，静态语句块只能访问到定义在静态语句块之前的变量，定义在它之后的变量，在前面的静态语句块可以赋值，但是不能访问
