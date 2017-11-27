# Dive Into Java
1. **java中的几种基本数据类型是什么，各自占了多少字节。**

   - 逻辑型

     boolean

   - 文本型

     char

   - 整数型

     byte short int long

   - 浮点型

     float double

   |   类型    |             字节数             |
   | :-----: | :-------------------------: |
   | boolean |    待定（java虚拟机规范写的是4个字节）     |
   |  char   | 2（java是unicode编码，一个字符占2个字节） |
   |  byte   |              1              |
   |  short  |              2              |
   |   int   |              4              |
   |  long   |              8              |
   |  float  |              4              |
   | double  |              8              |

2. **Java中是否可以继承String类，为什么？**

   不可以。String类有final修饰符。

3. **String，Stringbuffer，StringBuilder的区别**

   String：字符串常量，不可变的对象

   StringBuffer：字符串变量（线程安全）

   StringBuilder：字符串变量（非线程安全）

   一般情况下执性速度：StringBuilder>StringBuffer>String

4. **ArrayList 和 LinkedList 有什么区别？**

   - ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。 
   - 对于随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针。 
   - 对于新增和删除操作add和remove，LinedList比较占优势，因为ArrayList要移动数据。 

5. **讲讲类的实例化顺序，比如父类静态数据，构造函数，字段，子类静态数据，构造函数，字段，当 new 的时候，他们的执行顺序。**

   - 虚拟机在首次加载Java类时，会对静态初始化块、静态成员变量、静态方法(这三种统称为静态代码块)进行一次初始化 。
   - 类实例创建过程：按照父子继承关系进行初始化，首先执行父类的初始化块部分，然后是父类的构造方法；再执行本类继承的子类的初始化块，最后是子类的构造方法 。
   - 类实例销毁时候，首先销毁子类部分，再销毁父类部分。

   new一个对象时，执行过程如下：

   默认初始化（对成员赋0或NULL）—>进构造函数—>super()【父类也是该过程】—>显式初始化—>构造代码块—>构造函数中其它代码

6. **用过哪些 Map 类，都有什么区别。HashMap 是线程安全的吗？并发下使用的 Map 是什么，他们内部原理分别是什么，比如存储方式，hashcode，扩容，默认容量等。**

   - HashMap

     - 数据结构

       HashMap底层实现还是数组，只是数组的每一项都是一条链。每次新建一个HashMap时，都会初始化一个table数组。table数组的元素为Entry节点。Entry为HashMap的内部类，它包含了键key、值value、下一个节点next，以及hash值。

     - 默认容量

       初始容量16，默认加载因子0.75。容量表示哈希表中桶的数量，初始容量是创建哈希表时的容量，加载因子是哈希表在其容量自动增加之前可以达到多满的一种尺度，它衡量的是一个散列表的空间的使用程度。

     - 存储过程

       HashMap保存数据的过程为：首先判断key是否为null，若为null，则直接调用putForNullKey方法。若不为空则先计算key的hash值，然后根据hash值搜索在table数组中的索引位置，如果table数组在该位置处有元素，则通过比较是否存在相同的key，若存在则覆盖原来key的value，否则将该元素保存在链头（最先保存的元素放在链尾）。若table在该处没有元素，则直接保存。

     - 扩容

       该临界点在当HashMap中元素的数量等于table数组长度*加载因子。但是扩容是一个非常耗时的过程，因为它需要重新计算这些数据在新table数组中的位置并进行复制处理。

   - HashTable

     类似HashMap。还是有一些不同的：

     - HashTable基于Dictionary类，而HashMap是基于AbstractMap。Dictionary是什么？它是任何可将键映射到相应值的类的抽象父类，而AbstractMap是基于Map接口的骨干实现，它以最大限度地减少实现此接口所需的工作。
     - HashMap可以允许存在一个为null的key和任意个为null的value，但是HashTable中的key和value都不允许为null。

   - TreeMap

     TreeMap的实现是红黑树算法的实现。

     红黑树顾名思义就是节点是红色或者黑色的平衡二叉树，它通过颜色的约束来维持着二叉树的平衡。对于一棵有效的红黑树二叉树而言我们必须增加如下规则：

     - 每个节点都只能是红色或者黑色
     - 根节点是黑色
     - 每个叶节点（NIL节点，空节点）是黑色的。
     - 如果一个结点是红的，则它两个子节点都是黑的。也就是说在一条路径上不能出现相邻的两个红色结点。
     - 从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。

   - hashcode

     - hashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，hashCode是用来在散列存储结构中确定对象的存储地址的；
     - 如果两个对象相同，就是适用于equals(java.lang.Object) 方法，那么这两个对象的hashCode一定要相同；
     - 如果对象的equals方法被重写，那么对象的hashCode也尽量重写，并且产生hashCode使用的对象，一定要和equals方法中使用的一致，否则就会违反上面提到的第2点；
     - 两个对象的hashCode相同，并不一定表示两个对象就相同，也就是不一定适用于equals(java.lang.Object) 方法，只能够说明这两个对象在散列存储结构中，如Hashtable，他们“存放在同一个篮子里”。

   HashMap在并发执行put操作时会引起死循环，是因为多线程会导致HashMap的Entry链表形成环，一旦成环，Entry的next节点永远不为空，就会产生死循环。

   Hashtable是Java中最老的Map类，它是一个线程安全的Map类，其公有方法均使用synchronize关键字修饰,这表示在多线程操作时，每个线程在操作之前都会锁住整个map，待操作完成后才释放，如线程1使用put进行元素添加,线程2不但不能使用put方法进行添加元素,也不能使用get方法来获取元素,所以竞争越激烈效率越低，这必然导致多线程时性能不佳。另外，Hashtable不能使用null作为key或者value。

   ConcurrentHashMap的锁分段技术可有效提升并发访问率 。首先将数据分成一段一段地存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问。

7. **JAVA8 的 ConcurrentHashMap 为什么放弃了分段锁，有什么问题吗，如果你来设计，你如何设计。**

   取消segments字段，直接采用transient volatile HashEntry<K,V>[] table保存数据，采用table数组元素作为锁，从而实现了对每一行数据进行加锁，进一步减少并发冲突的概率。

   将原先table数组＋单向链表的数据结构，变更为table数组＋单向链表＋红黑树的结构。对于hash表来说，最核心的能力在于将key hash之后能均匀的分布在数组中。如果hash之后散列的很均匀，那么table数组中的每个队列长度主要为0或者1。但实际情况并非总是如此理想，虽然ConcurrentHashMap类默认的加载因子为0.75，但是在数据量过大或者运气不佳的情况下，还是会存在一些队列长度过长的情况，如果还是采用单向列表方式，那么查询某个节点的时间复杂度为O(n)；因此，对于个数超过8(默认值)的列表，jdk1.8中采用了红黑树的结构，那么查询的时间复杂度可以降低到O(logN)，可以改进性能。


8. **有没有有顺序的 Map 实现类，如果有，他们是怎么保证有序的。**

   TreeMap：可以指定排序函数，但不是按照插入顺序排序的。内部是红黑树的存储，可以实现排序。

   LinkedHashMap：内部有一个链表，保持插入的顺序。迭代的时候，也是按照插入顺序迭代，而且迭代比HashMap快。

9. **抽象类和接口的区别，类可以继承多个类么，接口可以继承多个接口么,类可以实现多个接口么。**

   - 抽象类和接口都不能直接实例化，如果要实例化，抽象类变量必须指向实现所有抽象方法的子类对象，接口变量必须指向实现所有接口方法的类对象。
   - 抽象类要被子类继承，接口要被类实现。
   - 接口只能做方法申明，抽象类中可以做方法申明，也可以做方法实现。
   - 接口里定义的变量只能是公共的静态的常量，抽象类中的变量是普通变量。
   - 抽象类里的抽象方法必须全部被子类所实现，如果子类不能全部实现父类抽象方法，那么该子类只能是抽象类。同样，一个实现接口的时候，如不能全部实现接口方法，那么该类也只能为抽象类。
   - 抽象方法只能申明，不能实现，接口是设计的结果 ，抽象类是重构的结果
   - 抽象类里可以没有抽象方法
   - 如果一个类里有抽象方法，那么这个类只能是抽象类
   - 抽象方法要被实现，所以不能是静态的，也不能是私有的。
   - 接口可继承接口，并可多继承接口，但类只能单根继承。

10. **继承和聚合的区别。**

  - 如果新的类只是使用老的类的一部分功能，那么就是用聚合，直接new出来就可以了，满足need或者组合这个关系；
  - 如果新的类必须是老的类的一个特殊例子，那么就是用继承，满足is-a或者is-like-a这种关系。

  继承

  指的是一个类（称为子类、子接口）继承另外的一个类（称为父类、父接口）的功能，并可以增加它自己的新功能的能力，继承是类与类或者接口与接口之间最常见的关系；在Java中此类关系通过关键字extends明确标识，在设计时一般没有争议性； 

  实现

  指的是一个class类实现interface接口（可以是多个）的功能；实现是类与接口之间最常见的关系；在Java中此类关系通过关键字implements明确标识，在设计时一般没有争议性； 

  依赖

  可以简单的理解，就是一个类A使用到了另一个类B，而这种使用关系是具有偶然性的、、临时性的、非常弱的，但是B类的变化会影响到A；比如某人要过河，需要借用一条船，此时人与船之间的关系就是依赖；表现在代码层面，为类B作为参数被类A在某个method方法中使用； 

  关联

  他体现的是两个类、或者类与接口之间语义级别的一种强依赖关系，比如我和我的朋友；这种关系比依赖更强、不存在依赖关系的偶然性、关系也不是临时性的，一般是长期性的，而且双方的关系一般是平等的、关联可以是单向、双向的；表现在代码层面，为被关联类B以类属性的形式出现在关联类A中，也可能是关联类A引用了一个类型为被关联类B的全局变量； 

  聚合

  聚合是关联关系的一种特例，他体现的是整体与部分、拥有的关系，即has-a的关系，此时整体与部分之间是可分离的，他们可以具有各自的生命周期，部分可以属于多个整体对象，也可以为多个整体对象共享；比如计算机与CPU、公司与员工的关系等；表现在代码层面，和关联关系是一致的，只能从语义级别来区分； 

  组合

  组合也是关联关系的一种特例，他体现的是一种contains-a的关系，这种关系比聚合更强，也称为强聚合；他同样体现整体与部分间的关系，但此时整体与部分是不可分的，整体的生命周期结束也就意味着部分的生命周期结束；比如你和你的大脑；表现在代码层面，和关联关系是一致的，只能从语义级别来区分； 

11. **谈谈nio的理解**

    阻塞I/O在调用InputStream.read()方法时是阻塞的，它会一直等到数据到来时（或超时）才会返回；同样，在调用ServerSocket.accept()方法时，也会一直阻塞到有客户端连接才会返回，每个客户端连接过来后，服务端都会启动一个线程去处理该客户端的请求。阻塞I/O通信模型，有两点缺点：

    - 当客户端多时，会创建大量的处理线程。且每个线程都要占用栈空间和一些CPU时间。
    - 阻塞可能带来频繁的上下文切换，且大部分上下文切换可能是无意义的。

    java NIO的工作原理：

    - 由一个专门的线程来处理所有的 IO 事件，并负责分发。
    - 事件驱动机制：事件到的时候触发，而不是同步的去监视事件。
    - 线程通讯：线程之间通过 wait,notify 等方式通讯。保证每次上下文切换都是有意义的。减少无谓的线程切换。

    java NIO采用了双向通道（channel）进行数据传输，而不是单向的流（stream），在通道上可以注册我们感兴趣的事件。比如：

    | 事件名          | 对应值                        |
    | ------------ | -------------------------- |
    | 服务端接收客户端连接事件 | SelectionKey.OP_ACCEPT(16) |
    | 客户端连接服务端事件   | SelectionKey.OP_CONNECT(8) |
    | 读事件          | SelectionKey.OP_READ(1)    |
    | 写事件          | SelectionKey.OP_WRITE(4)   |

    服务端和客户端各自维护一个管理通道的对象，我们称之为selector，该对象能检测一个或多个通道 (channel) 上的事件。我们以服务端为例，如果服务端的selector上注册了读事件，某时刻客户端给服务端发送了一些数据，阻塞I/O这时会调用read()方法阻塞地读取数据，而NIO的服务端会在selector中添加一个读事件。服务端的处理线程会轮询地访问selector，如果访问selector时发现有感兴趣的事件到达，则处理这些事件，如果没有感兴趣的事件到达，则处理线程会一直阻塞直到感兴趣的事件到达为止。

12. **反射的原理，反射创建类实例的三种方式是什么。**

    指在运行状态中，对于任意一个类,都能够知道这个类的所有属性和方法；对于任意一个对象,都能调用它的任意一个方法。这种动态获取信息，以及动态调用对象方法的功能叫java语言的反射机制。

    - 创建Class对象的方式一：(对象.getClass())
    - 创建Class对象的方式二：(类.class:需要输入一个明确的类)
    - 创建Class对象的方式三：(forName():传入时只需要以字符串的方式传入即可)。Class.forName()有异常：ClassNotFoundException

13. **反射中，Class.forName 和 ClassLoader 区别。**

    java中class.forName和classLoader都可用来对类进行加载。前者除了将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块。

    而classLoader只干一件事情，就是将.class文件加载到jvm中，不会执行static中的内容，只有在newInstance才会去执行static块。

    Class.forName(name, initialize, loader)带参函数也可控制是否加载static块。并且只有调用了newInstance()方法才去调用构造函数，创建类的对象 。

14. **描述动态代理的几种实现方式，分别说出相应的优缺点。**

    静态代理：由程序员创建或特定工具自动生成源代码，再对其编译。在程序运行前，代理类的.class文件就已经存在了。 
    动态代理：在程序运行时，运用反射机制动态创建而成。 

    动态代理有两种方式：

    - jdk方式

      委托类必须实现接口，代理类只能对接口进行代理。使用java的反射机制，以及Proxy和InvocationHandler来实现，代理类与委托类实现了相同的接口。 


    - cglib

      code generate library，代理类可对类进行代理，使用第三方cglib库来实现，其内部使用asm框架生成代理类的字节码，其字节码文件更加复杂，不能代理final方法，因为代理类是委托类的子类。 

    cglib生成的代理类的运行性能比jdk更加优秀，但是生成对象的时间更长，在单例模式中可以使用cglib，在频繁创建对象的模式中建议使用jdk。

15. **动态代理与 cglib 实现的区别。**

    参上。

16. **为什么 CGlib 方式可以对没有接口的类实现代理。**

    CGLib采用了非常底层的字节码技术，其原理是通过字节码技术为一个类创建子类，并在子类中采用方法拦截的技术拦截所有父类方法的调用，顺势织入横切逻辑。

17. **final的用途。**

    - final修饰类：

      被final修饰的类，是不可以被继承的，这样做的目的可以保证该类不被修改，Java的一些核心的API都是final类，例如String、Integer、Math等。

    - final修饰方法：

      子类不可以重写父类中被final修饰的方法。

    - final修饰实例变量(类的属性，定义在类内，但是在类内的方法之外)：
      final修饰实例变量时必须初始化，且不可再修改。

    - final修饰局部变量(方法体内的变量)：

      final修饰局部变量时只能初始化(赋值)一次，但也可以不初始化。

    - final修饰方法参数
      final修饰方法参数时，是在调用方法传递参数时候初始化的。

18. **写出三种单例模式实现。**

    - 延迟加载——不考虑效率问题的延迟加载

      ```java
      public class SingleTon{
          private static SingleTon instance = null;
          public static synchronized SingleTon getInstance(){
              if(instance==null){
                  instance = new SingleTon();
              }
          }
      }
      ```

      ​

    - 即时加载的基本实现

      ```java
      public class SingleTon(){
          private static SingleTon instance = new SingleTon();
          public static SingleTon getInstance(){
              return instance;
          }
      }
      ```

      ​

    - Double Check方式——关键考虑在于并发环境下返回结果的性能提升能否抵消多出来的两次判断跳转

      ```java
      public class SingleTon{
          private volatile static  SingleTon uniqueInstance;
          private SingleTon(){}
          public static SingleTon getInstance(){
              if(uniqueInstance==null){
                  synchronized(SingleTon.class){
                      if(uniqueInstance == null){
                          uniqueInstance = new SingleTon();
                      }
                  }
              }
              return uniqueInstance;
          }
      }
      ```

19. **如何在父类中为子类自动完成所有的 hashcode 和 equals 实现？这么做有何优劣。**

    不会写

20. **请结合 OO 设计理念，谈谈访问修饰符 public、private、protected、default 在应用设计中的作用。**

    - 单一职责原则

      每个类都只做一件事。

    - 里是替换原则

      定义：所有引用基类的地方必须能透明地使用其子类对象。

      1. 子类必须完全实现父类的方法
      2. 子类可以有自己的个性
      3. 覆盖或实现父类的方法时输入参数可以被放大：所有子类中的方法的前置条件必须与超类中被覆写的方法前置条件相同或者更宽松
      4. 覆写或实现父类的方法时，输出结果可以缩小

    - 依赖倒置原则

      - 模块间的依赖通过抽象发生，实现类之间不发生直接依赖关系，其依赖关系是通过接口或者抽象类产生的。
      - 接口或抽象类不依赖于实现类。
      - 实现类依赖接口或抽象类。

      面向接口编程。

    - 接口隔离原则

      接口有两种：

      - 实例接口：java声明的一个类，然后用new产生一个实例。
      - 类接口：interface关键字定义的接口。

      定义：

      - 客户端不应该依赖它不需要的接口。
      - 类之间的依赖关系应该建立在最小的接口上。

    - 迪米特法则

      也被称为最少知识原则。一个对象应该对其他对象有最少的了解。通俗来讲就是，被调用的类多复杂都没关系，只关心其对外暴露的public方法。

      实际应用中，如果一个类跳转两次以上才能访问到另一个类，就需要想办法重构了。

    - 开闭原则

      开闭原则是最基础的一个原则。一个软件实体如类、模块和函数应该对扩展开放，对修改关闭。

21. **深拷贝和浅拷贝区别。**

    将一个对象的引用复制给另外一个对象，一共有三种方式。第一种方式是直接赋值，第二种方式是浅拷贝，第三种是深拷贝。

    - 直接赋值

      在Java中，A a1 = a2，我们需要理解的是这实际上复制的是引用，也就是说a1和a2指向的是同一个对象。因此，当a1变化的时候，a2里面的成员变量也会跟着变化。

    - 浅拷贝

      Object的Clone()函数：创建一个新对象，然后将当前对象的非静态字段复制到该新对象，如果字段是值类型的，那么对该字段执行复制；如果该字段是引用类型的话，则复制引用但不复制引用的对象。因此，原始对象及其副本引用同一个对象。

    - 深拷贝

      深拷贝，就是说创建一个新对象，然后将当前对象的非静态字段复制到该新对象，无论该字段是值类型的还是引用类型，都进行复制。

      现在为了要在clone对象时进行深拷贝， 那么就要Clonable接口，覆盖并实现clone方法，除了调用父类中的clone方法得到新的对象， 还要将该类中的引用变量也clone出来。如果只是用Object中默认的clone方法，是浅拷贝的。

22. **数组和链表数据结构的描述，各自的时间复杂度。**

    - 结构描述：
      - 数组是在内存中开辟一段连续的空间，并在此空间存放元素。
      - 链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成。
    - 复杂度
      - 按序号查找时，数组可以随机访问，时间复杂度为O(1)，而链表不支持随机访问，平均需要O(n)； 
      - 按值查找时，若数组无序，数组和链表时间复杂度均为O(n)，但是当数组有序时，可以采用折半查找将时间复杂度降为O(logn)； 
      - 插入和删除时，数组平均需要移动n/2个元素，而链表只需修改指针即可； 

23.  **error 和 exception 的区别，CheckedException，RuntimeException 的区别。**

    - error 和 exception 的区别

      - Error类一般是指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢等。对于这类错误的导致的应用程序中断，仅靠程序本身无法恢复和和预防，遇到这样的错误，建议让程序终止。
      - Exception类表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常。

    - CheckedException，RuntimeException 的区别

      Exception类又分为运行时异常（Runtime Exception）和受检查的异常(Checked Exception )。

      - RuntimeException 运行时异常：ArithmaticException,IllegalArgumentException，编译能通过，但是一运行就终止了，程序不会处理运行时异常，出现这类异常，程序会终止。
      - CheckedException受检查的异常，要么用try。。。catch捕获，要么用throws字句声明抛出，交给它的父类处理，否则编译不会通过。

24. **请列出5个运行时异常。**

    - ClassCastException(类转换异常)
    - IndexOutOfBoundsException(数组越界)
    - NullPointerException(空指针)
    - ArrayStoreException(数据存储异常，操作数组时类型不一致)
    - BufferOverflowException(还有IO操作的,缓冲溢出异常)

25. **在自己的代码中，如果创建一个 java.lang.String 对象，这个对象是否可以被类加载器加载？**

    -  双亲委派模型

      从虚拟机角度看，只存在两种类加载器：1. 启动类加载器。2. 其他类加载器。从开发人员角度看，包括如下类加载器：1. 启动类加载器。2. 扩展类加载器。3. 应用程序类加载器。4. 自定义类加载器。

      - 启动类加载器，用于加载Java API，加载<JAVA_HOME>\lib目录下的类库。
      - 扩展类加载类，由sun.misc.Launcher$ExtClassLoader实现，用于加载<JAVA_HOME>\lib\ext目录下或者被java.ext.dirs系统变量指定路径下的类库。
      - 应用程序类加载器，也成为系统类加载器，由sun.misc.Launcher$AppClassLoader实现，用于加载用户类路径(ClassPath)上所指定的类库。
      - 自定义类加载器，继承系统类加载器，实现用户自定义加载逻辑。

      各个类加载器之间是组合关系，并非继承关系。

      当一个类加载器收到类加载的请求，它将这个加载请求委派给父类加载器进行加载，每一层加载器都是如此，最终，所有的请求都会传送到启动类加载器中。只有当父类加载器自己无法完成加载请求时，子类加载器才会尝试自己加载。

      双亲委派模型可以确保安全性，可以保证所有的Java类库都是由启动类加载器加载。如用户编写的java.lang.Object，加载请求传递到启动类加载器，启动类加载的是系统中的Object对象，而用户编写的java.lang.Object不会被加载。如用户编写的java.lang.virus类，加载请求传递到启动类加载器，启动类加载器发现virus类并不是核心Java类，无法进行加载，将会由具体的子类加载器进行加载，而经过不同加载器进行加载的类是无法访问彼此的。由不同加载器加载的类处于不同的运行时包。所有的访问权限都是基于同一个运行时包而言的。　

    - 自定义的String类不能被加载

      答案是否定的。我们不能实现。为什么呢？我看很多网上解释是说双亲委托机制解决这个问题，其实不是非常的准确。因为双亲委托机制是可以打破的，你完全可以自己写一个classLoader来加载自己写的java.lang.String类，但是你会发现也不会加载成功，具体就是因为针对java.*开头的类，jvm的实现中已经保证了必须由bootstrp来加载。

26. **说一说你对 java.lang.Object 对象中 hashCode 和 equals 方法的理解。在什么场景下需要重新实现这两个方法。**

    - equals

      Object类中默认的实现方式是  :   return this == obj  。那就是说，只有this 和 obj引用同一个对象，才会返回true。

      而我们往往需要用equals来判断 2个对象是否等价，而非验证他们的唯一性。这样我们在实现自己的类时，就要重写equals。重写equals方法时要遵循如下原则：

      - 自反性:  x.equals(x) 一定是true。
      - 对null:  x.equals(null) 一定是false。
      - 对称性:  x.equals(y)  和  y.equals(x)结果一致。
      - 传递性:  a 和 b equals , b 和 c  equals，那么 a 和 c也一定equals。
      - 一致性:  在某个运行时期间，2个对象的状态的改变不会不影响equals的决策结果，那么，在这个运行时期间，无论调用多少次equals，都返回相同的结果。

    - hashcode

      这个方法返回对象的散列码，返回值是int类型的散列码。对象的散列码是为了更好的支持基于哈希机制的Java集合类，例如 Hashtable, HashMap, HashSet 等。

      关于hashCode方法，一致的约定是：

      - 重写了euqls方法的对象必须同时重写hashCode()方法。
      - 如果2个对象通过equals调用后返回是true，那么这个2个对象的hashCode方法也必须返回同样的int型散列码。
      - 如果2个对象通过equals返回false，他们的hashCode返回的值允许相同。（hashCode返回独一无二的散列码，会让存储这个对象的hashtables更好地工作。）

      等价的(调用equals返回true)对象必须产生相同的散列码。不等价的对象，不要求产生的散列码不相同。

27. **在 jdk1.5 中，引入了泛型，泛型的存在是用来解决什么问题。**

    有许多原因促成了泛型的出现，而最引人注意的一个原因，就是为了创建容器类。

    **泛型类**

    容器类应该算得上最具重用性的类库之一。先来看一个没有泛型的情况下的容器类如何定义：

    ```java
    public class Container {
        private String key;
        private String value;

        public Container(String k, String v) {
            key = k;
            value = v;
        }
        
        public String getKey() {
            return key;
        }

        public void setKey(String key) {
            this.key = key;
        }

        public String getValue() {
            return value;
        }

        public void setValue(String value) {
            this.value = value;
        }
    }
    ```

    Container类保存了一对key-value键值对，但是类型是定死的，也就说如果我想要创建一个键值对是String-Integer类型的，当前这个Container是做不到的，必须再自定义。那么这明显重用性就非常低。

    当然，我可以用Object来代替String，并且在Java SE5之前，我们也只能这么做，由于Object是所有类型的基类，所以可以直接转型。但是这样灵活性还是不够，因为还是指定类型了，只不过这次指定的类型层级更高而已，有没有可能不指定类型？有没有可能在运行时才知道具体的类型是什么？

    所以，就出现了泛型。

    ```java
    public class Container<K, V> {
        private K key;
        private V value;

        public Container(K k, V v) {
            key = k;
            value = v;
        }

        public K getKey() {
            return key;
        }

        public void setKey(K key) {
            this.key = key;
        }

        public V getValue() {
            return value;
        }

        public void setValue(V value) {
            this.value = value;
        }
    }
    ```

    在编译期，是无法知道K和V具体是什么类型，只有在运行时才会真正根据类型来构造和分配内存。

    **泛型接口**

    在泛型接口中，生成器是一个很好的理解，看如下的生成器接口定义：

    ```java
    public interface Generator<T> {
        public T next();
    }
    ```

    然后定义一个生成器类来实现这个接口：

    ```java
    public class FruitGenerator implements Generator<String> {

        private String[] fruits = new String[]{"Apple", "Banana", "Pear"};

        @Override
        public String next() {
            Random rand = new Random();
            return fruits[rand.nextInt(3)];
        }
    }
    ```

    **泛型方法**

    一个基本的原则是：无论何时，只要你能做到，你就应该尽量使用泛型方法。也就是说，如果使用泛型方法可以取代将整个类泛化，那么应该有限采用泛型方法。下面来看一个简单的泛型方法的定义：

    ```java
    public class Main {

        public static <T> void out(T t) {
            System.out.println(t);
        }

        public static void main(String[] args) {
            out("findingsea");
            out(123);
            out(11.11);
            out(true);
        }
    }
    ```

    <T>是用来规范T的，例如<T extends Object>就规定了边界，即规定了所有出现T的地方，T类型必须是Object的子类。

    对于public <T> T fun() 和 public T fun()来说： 

    - public <T> T fun():在使用该方法时传入类型即可。
    - public T fun():在类实例化的时候就必须传入泛型参数。

28.  **a.hashcode() 有什么用，与 a.equals(b)有什么关系。**

    参照26

29. **有没有可能 2 个不相等的对象有相同的 hashcode。**

    参照26

30. **Java 中的 HashSet 内部是如何工作的。**

    对于HashSet而言，它是基于HashMap来实现的，底层采用HashMap来保存元素。

    HashSet继承AbstractSet类，实现Set、Cloneable、Serializable接口。其中AbstractSet提供 Set 接口的骨干实现，从而最大限度地减少了实现此接口所需的工作。Set接口是一种不包括重复元素的Collection，它维持它自己的内部排序，所以随机访问没有任何意义。

31. **什么是序列化，怎么序列化，为什么序列化，反序列化会遇到什么问题，如何解决。**

    **什么是序列化**

    序列化的都是对象在某一时刻的状态，也就是持久化对象这一时刻的状态；反序列化就是将这一对象状态解冻，反序列化出来的对象是一个新的对象实例。

    - 序列化：将java对象转换为可保持或可传输的格式，即转换为二进制字节序列（字节流）的形式的过程。
    - 反序列化：将二进制字节序列（字节流）形式的java对象解析出来的过程。

    **为什么序列化**

    当两个进程间进行通讯时可以相互发送任何资源（数据），这些资源包括图片、文本、音频、视频等，而这些资源在网络上传输的格式均为二进制序列（字节流）。
    当两个java进程**实现进程间的对象传输**时，必须完成对象的序列化，即将对象转换为二进制序列（字节流）的形式。

    **序列化的好处**

    - 序列化可以实现数据的持久化，也就是说可以将数据永久的保存在磁盘上。
    - 序列化可以实现远程通讯，即在网络上以二进制字节序列的格式传送对象。
    - 序列化可以将对象状态保存，方便下次取出利用。
    - 有了序列化，两个进程就可以在网络上任性的交互数据了。

    **怎样实现序列化**

    - 继承Serializable接口，使用默认的方式来进行序列化

      这种序列化方式仅仅对对象的非transient的实例变量进行序列化，而不会序列化对象的transient的实例变量，也不会序列化静态变量，所以我们对不想持久化的变量可以加上transient关键字。

      注意使用默认机制，在序列化对象时，不仅会序列化当前对象本身，还会对该对象引用的其它对象也进行序列化，同样地，这些其它对象引用的另外对象也将被序列化，以此类推。所以，如果一个对象包含的成员变量是容器类对象，而这些容器所含有的元素也是容器类对象，那么这个序列化的过程就会较复杂，开销也较大。

    - 继承Externalnalizable（Externalnalizable为Serializable的子类）接口，自身控制序列化的行为。

      使用Externalizable序列化时，在进行反序列化的时候，会重新实例化一个对象，然后再将被反序列化的对象的状态全部复制到这个新的实例化对象当中去，这也就是为什么会调用构造方法，也因此必须有一个无参构造方法供其调用，并且权限是public。

    **序列化版本号serialVersionUID**

    serialVersionUID的取值是Java运行时环境根据类的内部细节自动生成的。如果对类的源代码作了修改，再重新编译，新生成的类文件的serialVersionUID的取值有可能也会发生变化。

    类的serialVersionUID的默认值完全依赖于Java编译器的实现，对于同一个类，用不同的Java编译器编译，有可能会导致不同的serialVersionUID，也有可能相同。为了提高serialVersionUID的独立性和确定性，强烈建议在一个可序列化类中显示的定义serialVersionUID，为它赋予明确的值。

    **两种序列化方式的比较**

    方式一（默认序列化）还原出来的对象并没有调用任何构造器，就像是直接将对象还原出来一样，再赋予我们的引用（其实在反序列化的时候反序列化出来的对象已经进行了重构并实例化，当然不需要调用构造器啦）！当另一个线程反序列化Person对象时，必须保证反序列化的线程存在Person.class,否则报ClassNotFound异常。

    虽然Externalizable接口继承自Serializable接口，但他们的序列化机制是完全不同的，也就是说序列化继承此接口的话，Serializable所有序列化机制全部失效。

    使用Serializable的实现方式，在反序列化是不会调用任何的序列化对象构造器，而使用Externalizable是会调用一个无参构造方法的。

    **反序列化带来的问题**

    如果反序列化的时候，这些即将被反序列化的数据是我们特殊构造的，Java应用对用户输入，即不可信数据做了反序列化处理，那么攻击者可以通过构造恶意输入，让反序列化产生非预期的对象，非预期的对象在产生过程中就有可能带来任意代码执行。


# Dive Into Framework

1. Tomcat

   **tomcat结构**

   - Tomcat的目录结构

     - bin：该目录下存放的是二进制可执行文件。这里有启停tomcat的脚本或可执行文件。
     - conf：一些重要的配置文件。
       - server.xml：配置整个服务器信息。例如修改端口号，添加虚拟主机等；
       - tomcatusers.xml：存储tomcat用户的文件，这里保存的是tomcat的用户名及密码，以及用户的角色信息。可以按着该文件中的注释信息添加tomcat用户，然后就可以在Tomcat主页中进入Tomcat Manager页面了；
       - web.xml：部署描述符文件，这个文件中注册了很多MIME类型，即文档类型。这些MIME类型是客户端与服务器之间说明文档类型的。
     - lib：Tomcat的类库，里面是一大堆jar文件。如果需要添加Tomcat依赖的jar文件，可以把它放到这个目录中，当然也可以把应用依赖的jar文件放到这个目录中，这个目录中的jar所有项目都可以共享。
     - logs：这个目录中都是日志文件，记录了Tomcat启动和关闭的信息，如果启动Tomcat时有错误，那么异常也会记录在日志文件中。
     - webapps：存放web项目的目录，其中每个文件夹都是一个项目；如果这个目录下已经存在了目录，那么都是tomcat自带的。项目。其中ROOT是一个特殊的项目，在地址栏中没有给出项目目录时，对应的就是ROOT项目。
     - work：运行时生成的文件，最终运行的文件都在这里。通过webapps中的项目生成的！可以把这个目录下的内容删除，再次运行时会生再次生成work目录。当客户端用户访问一个JSP文件时，Tomcat会通过JSP生成Java文件，然后再编译Java文件生成class文件，生成的java和class文件都会存放到这个目录下。

   - tomcat架构

     Tomcat架构采用类似俄罗斯嵌套娃娃（译注：一层套一层）的设计方式。换句话说，就是一个容器包含另一个容器，而这个被包含的容器实体反过来再包含别的实体。
     启动Tomcat时，它运行的Java虚拟机（JVM）实例中包含一个服务器顶级元素，该元素代表了一个完整Tomcat服务器。一个Tomcat服务器通常只包含一个Service对象，这个对象是一种包含一个或多个连接器（Connector，如HTTP、HTTPS连接器）的结构化元素，正是这些Connector通过一个Catalina的Servlet引擎来处理传入的请求。

     引擎（Engine）表示Tomcat中处理请求的核心代码，并且它还支持在其下定义多个虚拟主机（Host）。虚拟主机允许Tomcat引擎在将配置在一台机器上的多个域名（如www.my-site.com、www.your-site.com）分割开来互不干扰。

     反过来，每个虚拟主机又可以支持多个web应用部署在它下边，这就是我们所熟知的上下文对象（Context）。上下文（Context）是使用由Servlet规范中指定的Web应用程序格式表示,不论是压缩过的war包形式的文件还是未压缩的目录形式。此外，上下文一般是在web.xml文件中配置，并且该配置是根据servlet规范定义的。

     从上下文角度看，在上下文中又可以部署多个servlet，并且每个servlet都会被一个包装组件所包含。

   **tomcat类加载机制**

   ![tomcat类加载图](http://images0.cnblogs.com/blog2015/449064/201506/141304597074685.jpg)

   当tomcat启动时，会创建几种类加载器：

   - Bootstrap引导类加载器 

     加载JVM启动所需的类，以及标准扩展类（位于jre/lib/ext下）

   - System系统类加载器 

     加载tomcat启动的类，比如bootstrap.jar，通常在catalina.bat或者catalina.sh中指定。位于tomcat/bin下。

   - Common通用类加载器 

     加载tomcat使用以及应用通用的一些类，位于tomcat/lib下，比如servlet-api.jar。

   - webapp 应用类加载器 

     每个应用在部署后，都会创建一个唯一的类加载器。该类加载器会加载位于WEB-INF/lib下的jar文件中的class 和 WEB-INF/classes下的class文件。

   当应用需要到某个类时，则会按照下面的顺序进行类加载：

   - 使用bootstrap引导类加载器加载
   - 使用system系统类加载器加载
   - 使用应用类加载器在WEB-INF/classes中加载
   - 使用应用类加载器在WEB-INF/lib中加载
   - 使用common类加载器在tomcat/lib中加载

   **问题**

   - 为什么java文件放在Eclipse中的src文件夹下会优先jar包中的class?

     因为Eclipse中的src文件夹中的文件java以及webContent中的JSP都会在tomcat启动时，被编译成class文件放在 WEB-INF/class 中。而Eclipse外部引用的jar包，则相当于放在 WEB-INF/lib 中。

   - 在tomcat/lib 以及 WEB-INF/lib 中放置了不同版本的jar包，此时就会导致某些情况下报加载不到类的错误。

   - 如果多个应用使用同一jar包文件，当放置了多份，就可能导致多个应用间出现类加载不到的错误。

2. tomcat如何调优

   **硬件性能调优**

   系统硬件性能直接影响tomcat的并发量，起决定作用的是CPU和MEM，CPU运行速度提升，会带来tomcat响应时间的缩短，mem大小决定工程需要内存的大小和工程的并发数量。

   **JVM调优**

   - jdk

   尽量选用版本较高的JVM，一般来说高版本产品在速度和效率上比低版本会有改进。

   - jvm参数

      Tomcat内存优化主要是对 tomcat 启动参数优化，我们可以在 tomcat 的启动脚本 catalina.sh 中设置 JAVA_OPTS参数。

     - 参数说明

       ```properties
       -server 启用jdk 的 server 版； 
       -Xms   java虚拟机初始化时的最小内存； 
       -Xmx  java虚拟机可使用的最大内存； 
       -XX:PermSize    内存永久保留区域 
       -XX:MaxPermSize   内存最大永久保留区域 
       -Xms=-Xmx=服务器内存*70%,如部署tomcat,jboss在同一台服务器-Xms=-Xmx=服务器内存*80%*1/4，现公司服务器内存一般都可以加到最大4G，所以可以采取以下配置，把以下参数添加到catalina.sh里面，
        JAVA_OPTS='-Xms1024m -Xmx4096m -XX:PermSize=256M -XX:MaxNewSize=256m-XX:MaxPermSize=256m'
         
       #-Xmx6000m                        :设置JVM最大可用内存为6000MB
       #-Xms6000m                        :设置JVM初始可用内存为6000MB
       #-Xmn2g                              :设置年轻代大小为2G
       #-Xss128k                             :设置每个线程的堆栈大小为128k
       #-XX:NewRatio=4                 :设置年轻代与年老代的比值为4
       #-XX:SurvivorRatio=4            :设置年轻代中Eden区与Survivor区的大小比值为4
       #-XX:PermSize=512m                    :设置堆栈永久区起始大小为512m
       #-XX:MaxPermSize=512m             :设置堆栈永久区最大大小为512m
       #-XX:MaxTenuringThreshold=0     :设置垃圾最大年龄为0
       #-XX:+UseParallelGC                     :选择垃圾收集器为并行收集器
       #-XX:ParallelGCThreads=8             :配置并行收集器的线程数
       #-XX:+UseParallelOldGC                :配置年老代垃圾收集方式为并行收集
       #-XX:+UseAdaptiveSizePolicy     :并行收集器会自动选择年轻代区大小和相应的Survivor区比例，以达到目标系统规定的最低响应时>间或者收集频率等，此值建议使用并行收集器时，一直打开。
       ```

     - 常用的一个JVM配置方式：

       ```properties
       JAVA_OPTS="
       -server 
       -Xms1024m
       -Xmx1024m
       -Xmn384m
       -XX:PermSize=64m
       -XX:MaxPermSize=128m
       -XX:+UseParallelOldGC
       -XX:+PrintGCDateStamps
       -XX:+PrintGCDetails
       -Xloggc:/opt/tomcat/log/gc.log
       -XX:+HeapDumpOnOutOfMemoryError
       -XX:HeapDumpPath=/opt/tomcat/heap.bin"
       ```
       说明：

       - Xms与Xmx普遍选择配置相同的大小，实际大小根据实际情况调整，由小向大增加，没必要一开始就增加到很大的内存。
       - XX:PermSize设置堆栈永久区起始大小，XX:MaxPermSize设置堆栈永久区最大大，其实设置比默认值大写即可，或者默认也可以。这两个值，是因为出现过永久区内存溢出，才进行设定的。
       - UseParallelOldGC、PrintGCDateStamps、PrintGCDetails、Xloggc:/opt/tomcat/log/gc.log设置GClog日志，这个对分析tomcat中JVM内存使用情况非常有效。
       - XX:+HeapDumpOnOutOfMemoryError、XX:HeapDumpPath=/opt/tomcat/heap.bin"设置内存溢出时，输出HeapDump，具体如何使用分析HeapDump文件，请参考：http://vekergu.blog.51cto.com/9966832/1619640
       - 调整合适的JVM内存大小，开启GClog和HeapDump即可。

   - tomcat集群之session共享

      在{TOMCST_HOME}/conf/server.xml取消下面代码注释即可：

      ```xml
      <ClusterclassName="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
      ```

   - 站点的默认网页、自定义错误页面、禁止列目录等功能。

      - 默认主页

        ```xml
        <welcome-file-list>
        	<welcome-file>index.html</welcome-file>
            <welcome-file>index.htm</welcome-file>
            <welcome-file>index.jsp</welcome-file>
        </welcome-file-list>
        ```

      - 自定义错误页面

        ```xml
        <error-page> 
        	<error-code>404</error-code> 
        	<location>/404.htm</location> 
        </error-page>
        ```

      -  定义会话超时时间

        ```xml
        <session-config>
        	<session-timeout>30</session-timeout>
        </session-config>
        ```

      -  禁止列目录

        ```xml
        <init-param>
        	<param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        ```

      - 管理AJP端口

        AJP是为 Tomcat 与 HTTP 服务器之间通信而定制的协议，能提供较高的通信速度和效率。如果tomcat前端放的是apache的时候，会使用到AJP这个连接器。如果公司前端是由nginx做的反向代理，因此不使用此连接器，因此需要注销掉该连接器。在{TOMCST_HOME}/conf/server.xml中找到下列代码，注释即可。

        ```xml
        <!--
           <Connector port="8009" protocol="AJP/1.3"redirectPort="8443" />
        -->
        ```

      - 取消默认gc监听

        如果开启了GClog，再开启GC监听，会影响GClog输出，功能重复，可以选择取消。

        ```xml
        <!-- 内存泄露侦测，对于垃圾回收不能处理的对像，它就会做日志，开启gcc后，不需要这个功能-->
         <!-- ListenerclassName="org.apache.catalina.core.JreMemoryLeakPreventionListener"gcDaemonProtection="false"/ -->
        ```

      - 自定义tomcat代码路径

        ```xml
        <Context docBase="/code_path/code_file"path="/code_file " reloadable="false" />
        ```

        其中：docBase这个是你代码的路径，path这个是你访问网站的URL路径。

        reloadable如果为true，会自动加载变化的动态文件，看起来挺智能的，但是，在tomcat加载变化代码的时候有可能会出现内存溢出，tomcat服务不正常等异常，建议还是false掉，更新完代码脚本重启tomcat才是王道。

   - 性能优化

      - 屏蔽DNS查询

        Web应用程序可以通过Web容器提供的getRemoteHost()方法获得访问Web应用客户的IP地址和名称，但是这样会消耗Web容器的资源，并且还需要通过IP地址和DNS服务器反查用户的名字。因此当系统上线时，可以将这个属性关闭，从而减少资源消耗，那么Web应用也就只能记录下IP地址。

        修改的属性是enableLoopups="false"。

      - 调整线程数

        Tomcat通过线程池来为用户访问提供响应，对于上线的系统初步估计用户并发数量后，再调整线程池容量。例如，用户并发数量在100左右时，可以设置minProcessors="100"，maxProcessors="100"。

        将最大和最小设置为一样后，线程池不会再释放空闲的线程，当用户访问突然增加时，不需要再消耗系统资源去创建新的线程。

      - 调整最大连接数

        这个其实最复杂，即使用户并发量大，但是系统反应速度快，也没必要把这个值设置太高，高了系统需要消耗大量的资源去切换线程，但是如果设置太低也会造成应用无法满足用户并发需要。因此设置这个最好能够结合整个系统的跟踪与调优，使系统达到最好的平稳状态，

        一般设置为maxProcessors的1.5倍即可。

      - 调整网络超时

        主要是HTTP协议也有个连接过程，客户端连接到服务器上后，如果长时间没有得到处理就会被释放。如果服务器处理速度较慢，但是希望每个用户都能得到有效处理，或者网络环境不好，需要保证用户不会因为超时中断，也可以把时间加长。

        一般设置成connectionTimeout="30000"即可。太长对系统来说价值不大，反而会浪费系统资源在无谓的长连接上。

      - 压缩管理

         tomcat作为一个应用服务器，也是支持 gzip 压缩功能的。我们可以在 server.xml 配置文件中的 Connector 节点中配置如下参数，来实现对指定资源类型进行压缩。

        ```properties
        compression="on"            # 打开压缩功能
        compressionMinSize="50"     # 启用压缩的输出内容大小，默认为2KB
        noCompressionUserAgents="gozilla, traviata"      # 对于以下的浏览器，不启用压缩
        compressableMimeType="text/html,text/xml,text/javascript,text/css,text/plain"# 哪些资源类型需要压缩
        ```

        如果使用apache/nginx代理，所以tomcat自身不需要进行压缩，会给服务器增加压力。

      -  tomcat的三种运行模式

        - BIO

          默认的模式,性能非常低下,没有经过任何优化处理和支持。

        - NIO

          直接修改server.xml里的Connector节点,修改protocol为:

          ```xml
          <Connector port="80" 
                     protocol="org.apache.coyote.http11.Http11NioProtocol"
                     connectionTimeout="20000"
                     URIEncoding="UTF-8"
                     useBodyEncodingForURI="true"
                     enableLookups="false"
                     redirectPort="8443"/>
          ```

        - APR

          安装起来最困难,但是从操作系统级别来解决异步的IO问题,大幅度的提高性能.。必须要安装apr和native，直接启动就支持apr。

          安装成功后还需要对tomcat设置环境变量，方法是在catalina.sh文件中增加一 行：

          ```properties
          CATALINA_OPTS="-Djava.library.path=/usr/local/apr/lib"
          修改8080端对应的
          protocol="org.apache.coyote.http11.Http11AprProtocol"
          ```

      - Tomcat连接相关参数总结

        在Tomcat 配置文件 server.xml 中的 <Connector ... /> 配置中

        ```xml
        maxThreads         客户请求最大线程数
        minSpareThreads    Tomcat初始化时创建的 socket 线程数
        maxSpareThreads    Tomcat连接器的最大空闲 socket 线程数
        enableLookups      若设为true, 则支持域名解析，可把 ip 地址解析为主机名
        redirectPort        在需要基于安全通道的场合，把客户请求转发到基于SSL 的 redirectPort端口
        acceptAccount       监听端口队列最大数，满了之后客户请求会被拒绝（不能小于maxSpareThreads  ）
        connectionTimeout   连接超时
        minProcessors         服务器创建时的最小处理线程数
        maxProcessors        服务器同时最大处理线程数
        URIEncoding          URL统一编码
        compression          打开压缩功能  
        compressionMinSize   启用压缩的输出内容大小，这里面默认为2KB
        compressableMimeType 压缩类型
        connectionTimeout    定义建立客户连接超时的时间. 如果为 -1, 表示不限制建立客户连接的时间
        ```

        生产配置实例：

        ```xml
        <Connector port="8080" protocol="org.apache.coyote.http11.Http11NioProtocol"   #nio  利用java的异步io护理技术,noblocking IO技术.
                URIEncoding="UTF-8"    #设置编码
                minSpareThreads="25"  #Tomcat初始化时创建的 socket线程数
                maxSpareThreads="75"  #Tomcat连接器的最大空闲socket 线程数，一旦创建的线程超过这个值，Tomcat就会关闭不再需要的socket线程。默认值50
                enableLookups="false"  #屏蔽DNS查询
                disableUploadTimeout="true"  #该标志位表明当执行servlet时，是否允许servlet容器使用一个不同的、更长的连接超时。启用该标志位将导致在上传数据时，要么使用更长的时间完成上传，要么出现更长的超时。如果不指定，该属性为“false”。       
                connectionTimeout="20000"   #网络超时时间
                acceptCount="300"     #容许的最大连接数，一般设置为maxProcessors的1.5倍即可，满了之后客户请求会被拒绝（不能小于maxSpareThreads  ）
                maxThreads="300"     #客户请求最大线程数，默认值为“200”
                maxProcessors="1000"   #最大连接线程数，即：并发处理的最大请求数，默认值为75 ，一旦创建的线程超过这个值，Tomcat就会关闭不再需要的socket线程
                minProcessors="5"      #最小空闲连接线程数，用于提高系统处理性能，默认值为10
                useURIValidationHack="false"
                <!--   前端使用nginx作为反向代理，不需要启用tomcat压缩功能。
                compression="on"   #打开压缩功能
                compressionMinSize="2048" #启用压缩的输出内容大小，这里面默认为2KB
               compressableMimeType="text/html,text/xml,text/javascript,text/css,text/plain"   #压缩类型
                -->
                redirectPort="8443"/>
        ```

   - 安全优化

      - 隐藏版本信息

        Tomcat 安装目录下的lib目录下，名称为 catalina.jar，直接修改catalina.jar中的文件，org/apache/catalina/util/ServerInfo.properties

      - 禁用 Tomcat 管理页面

        这些页面是存放在 Tomcat 安装目录下的webapps目录下的。我们只需要删除该目录下的所有文件即可。当然，还有涉及管理页面的2个配置文件host-manager.xml 和 manager.xml 也需要一并删掉。这两个文件存放在 Tomcat 安装目录下的conf/Catalina/localhost目录下。

      - 用普通用户启动 Tomcat

        使用专用用户 tomcat 或者 nobody 用户来启动 Tomcat。在启动之前，需要对我们的tomcat 安装目录下所有文件的属主和属组都设置为指定用户。

      - 分离 Tomcat 和项目的用户

        为了防止 Tomcat 被植入 web shell 程序后，可以修改项目文件。因此我们要将 Tomcat 和项目的属主做分离，这样即便被搞，他也无法创建和编辑项目文件。

      - 关闭war自动部署

        默认 Tomcat 是开启了对war包的热部署的。为了防止被植入木马等恶意程序，因此我们要关闭自动部署。

        ```xml
         <Host name="localhost" appBase="" npackWARs="false" autoDeploy="false">
        ```

      - 更改关闭 Tomcat 实例的指令

         server.xml中定义了可以直接关闭 Tomcat 实例的管理端口。我们通过 telnet 连接上该端口之后，输入 SHUTDOWN （此为默认关闭指令）即可关闭 Tomcat 实例（注意，此时虽然实例关闭了，但是进程还是存在的）。由于默认关闭Tomcat 的端口和指令都很简单。默认端口为8005，指令为SHUTDOWN 。因此我们需要将关闭指令修改复杂一点。

        当然，在新版的 Tomcat 中该端口仅监听在127.0.0.1上，因此大家也不必担心。除非黑客登陆到tomcat本机去执行关闭操作。

        ```xml
        <Server port="8005" shutdown="9SDfjsd29jf24sdff0LSDdfJKS9DKkjsd">
        ```

   - 内核优化

      在修改内核参数的时候，建议大家逐个设置，然后反复试验，切勿图方便直接拿上就用，一次全部替换。

      优化网络参数，修改/etc/sysctl.cnf文件，在最后追加如下内容：

      ```properties
      net.core.netdev_max_backlog = 32768
      net.core.somaxconn = 32768
      net.core.wmem_default = 8388608
      net.core.rmem_default = 8388608
      net.core.rmem_max = 16777216
      net.core.wmem_max = 16777216
      net.ipv4.ip_local_port_range = 1024 65000
      net.ipv4.route.gc_timeout = 100
      net.ipv4.tcp_fin_timeout = 30
      net.ipv4.tcp_keepalive_time = 1200
      net.ipv4.tcp_timestamps = 0
      net.ipv4.tcp_synack_retries = 2
      net.ipv4.tcp_syn_retries = 2
      net.ipv4.tcp_tw_recycle = 1
      net.ipv4.tcp_tw_reuse = 1
      net.ipv4.tcp_mem = 94500000 915000000927000000
      net.ipv4.tcp_max_orphans = 3276800
      net.ipv4.tcp_max_syn_backlog = 65536
      ```

   - tomcat监控

      - 配置tomcat的status状态页

        步骤1：修改%tomcat安装路径%\conf \tomcat-users文件，配置admin设置权限。在<tomcat-users>中增加部分内容。具体如下：

        ```xml
        <role rolename="manager-gui"/>
        <user username="manager"password="1234" roles="manager-gui"/>
        ```

        步骤2：完成后，启动tomcat，输入：http://localhost:8080  --(IP,端口号，可远程访问)

        点击status,输入账号，密码（manager,1234），进入status，时时刷新页面，查看当前tomcat状态。 或者直接访问：http://localhost:8080/manager/status页面。

      -  tomcat监控脚本

        ```bash
        #!/bin/bash
        #auther：gushao
        #time：2015-5-4
        #========================DefaultSettings===============================
        # address of status page
        STATUS_ADDR="http://localhost:8080/manager/status?XML=true"
        USER="manager"
        PASS="1234"
        # sample rate, default: 5seconds
        SAMPLE_RATE=5
        # if press "Ctrl+c", stop monitor
        EXIT_SIGNAL=2
        # connector to monitor
        CONNECTOR="http-8080"
        # result directory to store data
        RESULT_DIR="/tmp"
        # perf data file
        PERF_DATA="perf_data"
        # jvm data file
        JVM_DATA="jvm_data"
        # connector data file
        CONNECTOR_DATA="connector_data"
        # thread data file
        THREAD_DATA="thread_data"
        # request data file
        REQUEST_DATA="request_data"
          
        # ===========================Output ErrorMessage========================
        # Show Error Message and exit, get oneparameter
        errorMsg()
        {
               if [[ $# -eq 1 ]]; then
                        echo "Runtime Error:$1"
                        exit 1
               else
                       echo "Function Error:errorMsg"
                        exit 127
               fi
        }
          
        # =========================Get DataFunction=============================
        # Get performance data, no parameter wanted
        getPerfData()
        {
               cd $RESULT_DIR
               wget --http-user="$USER" --http-password="$PASS""$STATUS_ADDR" -O "$PERF_DATA" || errorMsg "Failed toget data, please check the connection"
               # JVM data
               sed 's/.*<jvm>//g;s/<\/jvm>.*//g' $PERF_DATA | awk -F \' '{print $2, $4, $6 }' >> $JVM_DATA
               # 'Connector data
               sed 's/.*'$CONNECTOR'.>//g;s/<\/connector>.*//g' $PERF_DATA>> $CONNECTOR_DATA
               # Thread data
               sed 's/.*<threadInfo//g;s/\/>.*//g' $CONNECTOR_DATA | awk -F\" '{ print $2, $4, $6 }' >> $THREAD_DATA
               # " Request data
               sed 's/.*<requestInfo//g;s/\/>.*//g' $CONNECTOR_DATA | awk -F\" '{ print $2, $4, $6, $8, $10, $12 }' >> $REQUEST_DATA
          
        }
          
        # ========================Build ChartFunction==========================
        # "according the data, build the chart(use gnuplot)
        buildChart()
        {   
               TITLE=""
               OUTPUT=""
               PLOT=""
               YRANGE="[0:]"
               case "$1" in
                        "jvm" )
                        TITLE="JVM"
                       OUTPUT="jvm_graph.png"
                        PLOT="plot 'jvm_data'using 1 title 'free' w linespoints, \
                        'jvm_data' using 2 title'total' w linespoints,\
                        'jvm_data' using 3 title 'max'w linespoints"
                        ;;
          
                        "thread" )
                        TITLE="Thread"
                       OUTPUT="thread_graph.png"
                        PLOT="plot 'thread_data'using 1 title 'max threads' w linespoints,\
                        'thread_data' using 2 title'current thread count' w linespoints,\
                        'thread_data' using 3 title'current thread busy' w linespoints"
                        ;;
          
                        "request" )
                        TITLE="Request"
                        YRANGE="[-1:]"
                       OUTPUT="request_graph.png"
                        PLOT="plot 'request_data'using 1 title 'max time' w linespoints,\
                        'request_data' using 2 title'processing time' w linespoints,\
                        'request_data' using 3 title'request count' w linespoints,\
                        'request_data' using 4 title'error count' w linespoints,\
                        'request_data' using 5 title'bytes received' w linespoints,\
                        'request_data' using 6 title'bytes sent' w linespoints"
                        ;;
               esac
          
               # build graph
               gnuplot <<EOF
               set terminal png small size 480,360
               set title "$TITLE"
               set yrange $YRANGE
               set grid
               set xlabel "timeline (s)"
               set output "$OUTPUT"
               $PLOT
        EOF
        }
          
        # ========================Build ReportFunction=========================
        # include data and chart, give a readablehtml report
        buildReport()
        {
               # build graph jvm, request,thread
               buildChart "jvm" || errorMsg "Function Error: build jvmgraph"
               buildChart "thread" || errorMsg "Function Error: buildthread graph"
               buildChart "request" || errorMsg "Function Error: buildrequest graph"
               # build html report
        }
          
        # ========================Stop MonitorFunction
        # call buildReport function
        stopMonitor()
        {
           echo "Monitor stopped, and we are building the report ..."
           buildReport || errorMsg "Function Error: buildReport"
           exit
        }
          
        # =============================MainFunction=============================
        trap "stopMonitor" $EXIT_SIGNAL
        #收到stopMonitor信号，则中断退出
        while :
        do
               getPerfData || errorMsg "Failed to get performance data"
               sleep $SAMPLE_RATE
        done
        ```

3. **Spring加载流程**

   容器是spring的核心。spring容器负责创建对象，装配他们，配置并管理他们的整个生命周期。

   spring的容器一般分为bean工厂和应用上下文两种，由于bean工厂的比较低级，一般都是选择应用上下文的容器。

   **应用上下文**

   比较常用的几个是：

   - AnnotationConfigApplicationContext：java配置类中加载spring应用上下文
   - AnnotationConfigWebApplicationContext：java配置类中加载spring web应用上下文
   - ClassPathXmlApplicationContext：从类路径下加载
   - FileSystemXmlApplicationContext：从文件系统中加载
   - XmlWebApplicationContext：从web应用下加载

   **bean生命周期**

   1. spring对bean实例化。
   2. 将值和bean的引用注入到bean对应的属性中。
      - 设置名字：如果bean实现了BeanNameAware接口，spring将bean的id传递给setBeanName方法，设置bean的名字。
      - 设置容器：
        - 如果bean实现了BeanFactoryAware接口，spring将BeanFactory容器实例传入。
        - 如果bean实现了ApplicationContextAware接口，spring将应用上下文的引用传入。
      - 调用初始化方法
        - 如果实现了BeanPostProcessor接口，调用postProcessBeforeInitialization方法，最后还要调用postProcessAfterInitialization方法。
        - 如果实现了InitializationBean接口，调用afterPropertiesSet方法。还有一些其他的初始化方法。。
   3. bean已经就绪，可以被使用了。
   4. 销毁bean。

   **bean的作用域**

   - 单例singleton：在整个应用中只创建一个bean实例。
   - 原型prototype：每次注入或者通过spring上下文获取的时候，都创建一个bean实例。
   - 会话session：在web应用中，为每个会话创建一个bean实例。
   - 请求request：在web应用中，为每个请求创建一个bean实例。

4. **spring事务的传播属性**

   spring在TransactionDefinition中规定了7中类型的事务传播行为：

   - REQUIRED：

     默认的spring事务传播级别，使用该级别的特点是，如果上下文中已经存在事务，那么就加入到事务中执行，如果当前上下文中不存在事务，则新建事务执行。

   - SUPPORTS ：

     从字面意思就知道，supports，支持，该传播级别的特点是，如果上下文存在事务，则支持事务加入事务，如果没有事务，则使用非事务的方式执行。

     这个通常是用来处理那些并非原子性的非核心业务逻辑操作。应用场景较少

   - MANDATORY ：

     该级别的事务要求上下文中必须要存在事务，否则就会抛出异常！

     配置该方式的传播级别是有效的控制上下文调用代码遗漏添加事务控制的保证手段。比如一段代码不能单独被调用执行，但是一旦被调用，就必须有事务包含的情况，就可以使用这个传播级别。

   - REQUIRES_NEW：

     从字面即可知道，new，每次都要一个新事务。

     该传播级别的特点是，每次都会新建一个事务，并且同时将上下文中的事务挂起，执行当前新建事务完成以后，上下文事务恢复再执行。

     这是一个很有用的传播级别，举一个应用场景：现在有一个发送100个红包的操作，在发送之前，要做一些系统的初始化、验证、数据记录操作，然后发送100封红包，然后再记录发送日志，发送日志要求100%的准确，如果日志不准确，那么整个父事务逻辑需要回滚。

     怎么处理整个业务需求呢？就是通过这个PROPAGATION_REQUIRES_NEW 级别的事务传播控制就可以完成。发送红包的子事务不会直接影响到父事务的提交和回滚。

   - NOT_SUPPORTED：

     这个也可以从字面得知，not supported ，不支持，当前级别的特点就是上下文中存在事务，则挂起事务，执行当前逻辑，结束后恢复上下文的事务。

   - NEVER：

     该事务更严格，上面一个事务传播级别只是不支持而已，有事务就挂起，而PROPAGATION_NEVER传播级别要求上下文中不能存在事务，一旦有事务，就抛出runtime异常，强制停止执行！

   - NESTED：

     字面也可知道，nested，嵌套级别事务。该传播级别特征是，如果上下文中存在事务，则嵌套事务执行，如果不存在事务，则新建事务。

5. spring如何管理事务

   **数据库事务**

   数据库事务满足4个特性：

   - A 原子性：一个事务的数据库操作不可分割。
   - C 一致性：事务操作成功后，数据库所处的状态和业务规则一致。
   - I 隔离性：并发操作时，不同的事务之间不会干扰。
   - D 持久性：一旦事务提交成功，所有数据操作都被持久化到数据库中。

   一般使用执行日志来保证ACD，用锁机制保证I。

   **数据并发的问题**

   - 脏读 

     所谓的脏读，其实就是读到了别的事务回滚前的脏数据。比如事务B执行过程中修改了数据X，在未提交前，事务A读取了X，而事务B却回滚了，这样事务A就形成了脏读。

   - 不可重复读 

     不可重复读字面含义已经很明了了，比如事务A首先读取了一条数据，然后执行逻辑的时候，事务B将这条数据改变了，然后事务A再次读取的时候，发现数据不匹配了，就是所谓的不可重复读了。

   - 幻读 ：

     事务A首先根据条件索引得到10条数据，然后事务B改变了数据库一条数据，导致也符合事务A当时的搜索条件，这样事务A再次搜索发现有11条数据了，就产生了幻读。

   **数据隔离的级别**

   - Serializable ：

     最严格的级别，事务串行执行，资源消耗最大；

   - REPEATABLE READ ：

     保证了一个事务不会修改已经由另一个事务读取但未提交（回滚）的数据。避免了“脏读取”和“不可重复读取”的情况，但是带来了更多的性能损失。

   - READ COMMITTED：

     大多数主流数据库的默认事务等级，保证了一个事务不会读到另一个并行事务已修改但未提交的数据，避免了“脏读取”。该级别适用于大多数系统。

   - Read Uncommitted ：

     保证了读取过程中不会读取到非法数据。

   **spring事务管理**

   spring采用声明式事务管理，提供了一致的编程模板，因此使用spring jdbc或者mybatis都可以使用统一的编程模型。

   通过TransactionManager相关的实现类来委托给底层具体的持久化ORM框架来完成。

6. spring怎么配置事务

   **mybatis和spring jdbc**

   基于数据源的Connection来访问数据库，所以使用DataSourceTransactionManager。

   ```
   <bean id="dataSource" ... />
   <bean id="transactionManager" class="..." p:dataSource-ref="dataSource" />
   ```
   **Hibernate**

   ```
   <bean id="sessionFactory" ... />
   <bean id="transactionManager" class="..." p:sessionFactory-ref="sessionFactory" />
   ```




