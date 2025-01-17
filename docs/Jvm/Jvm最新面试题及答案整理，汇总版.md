# Jvm最新面试题及答案整理，汇总版

### 其实，博主还整理了，更多大厂面试题，直接下载吧

### 下载链接：[高清172份，累计 7701 页大厂面试题  PDF](https://github.com/souyunku/DevBooks/blob/master/docs/index.md)



### 1、动态改变构造

OSGi 服务平台提供在多种网络设备上无需重启的动态改变构造的功能。为了最小化耦合度和促使这些耦合度可管理， OSGi 技术提供一种面向服务的架构，它能使这些组件动态地发现对方。


### 2、怎么看死锁的线程？

通过jstack命令，可以获得线程的栈信息。死锁信息会在非常明显的位置（一般是最后）进行提示。


### 3、Java对象的布局了解过吗？

对象头区域此处存储的信息包括两部分：1、对象自身的运行时数据( MarkWord )，占8字节 存储 hashCode、GC 分代年龄、锁类型标记、偏向锁线程 ID 、 CAS 锁指向线程 LockRecord 的指针等， synconized 锁的机制与这个部分( markwork )密切相关，用 markword 中最低的三位代表锁的状态，其中一位是偏向锁位，另外两位是普通锁位。2、对象类型指针( Class Pointer )，占4字节 对象指向它的类元数据的指针、 JVM 就是通过它来确定是哪个 Class 的实例。

实例数据区域 此处存储的是对象真正有效的信息，比如对象中所有字段的内容

对齐填充区域 JVM 的实现 HostSpot 规定对象的起始地址必须是 8 字节的整数倍，换句话来说，现在 64 位的 OS 往外读取数据的时候一次性读取 64bit 整数倍的数据，也就是 8 个字节，所以 HotSpot 为了高效读取对象，就做了"对齐"，如果一个对象实际占的内存大小不是 8byte 的整数倍时，就"补位"到 8byte 的整数倍。所以对齐填充区域的大小不是固定的。


### 4、JVM 的内存模型是什么？

JVM 试图定义一种统一的内存模型，能将各种底层硬件以及操作系统的内存访问差异进行封装，使 Java 程序在不同硬件以及操作系统上都能达到相同的并发效果。它分为工作内存和主内存，线程无法对主存储器直接进行操作，如果一个线程要和另外一个线程通信，那么只能通过主存进行交换。


### 5、OSGI（ 动态模型系统）

OSGi(Open Service Gateway Initiative)，是面向 Java 的动态模型系统，是 Java 动态化模块化系统的一系列规范。


### 6、JVM的引用类型有哪些？

引用内型：

**强引用：**

当内存不足的时候，JVM宁可出现OutOfMemoryError错误停止，也需要进行保存，并且不会将此空间回收。在引用期间和栈有联系就无法被回收

**软引用：**

当内存不足的时候，进行对象的回收处理，往往用于高速缓存中；mybatis就是其中

**弱引用：**

不管内存是否紧张，只要有垃圾了就立即回收

**幽灵引用：**

和没有引用是一样的


### 7、你熟悉哪些垃圾收集算法？

标记清除（缺点是碎片化） 复制算法（缺点是浪费空间） 标记整理算法（效率比前两者差） 分代收集算法（老年代一般使用“标记-清除”、“标记-整理”算法，年轻代一般用复制算法）


### 8、JVM 选项 -XX:+UseCompressedOops 有什么作用？为什么要使用

当你将你的应用从 32 位的 JVM 迁移到 64 位的 JVM 时，由于对象的指针从32 位增加到了 64 位，因此堆内存会突然增加，差不多要翻倍。这也会对 CPU缓存（容量比内存小很多）的数据产生不利的影响。因为，迁移到 64 位的 JVM主要动机在于可以指定最大堆大小，通过压缩OOP 可以节省一定的内存。通过-XX:+UseCompressedOops 选项，JVM 会使用 32 位的 OOP，而不是 64 位的 OOP。


### 9、类初始化的情况有哪些？

1.
遇到 `new`、`getstatic`、`putstatic` 或 `invokestatic` 字节码指令时，还未初始化。典型场景包括 new 实例化对象、读取或设置静态字段、调用静态方法。

2.
对类反射调用时，还未初始化。

3.
初始化类时，父类还未初始化。

4.
虚拟机启动时，会先初始化包含 main 方法的主类。

5.
使用 JDK7 的动态语言支持时，如果 MethodHandle 实例的解析结果为指定类型的方法句柄且句柄对应的类还未初始化。

6.
口定义了默认方法，如果接口的实现类初始化，接口要在其之前初始化。

7.
其余所有引用类型的方式都不会触发初始化，称为被动引用。被动引用实例：① 子类使用父类的静态字段时，只有父类被初始化。② 通过数组定义使用类。③ 常量在编译期会存入调用类的常量池，不会初始化定义常量的类。

8.
接口和类加载过程的区别：初始化类时如果父类没有初始化需要初始化父类，但接口初始化时不要求父接口初始化，只有在真正使用父接口时（如引用接口中定义的常量）才会初始化。



### 10、分代收集算法

当前主流 VM 垃圾收集都采用”分代收集” (Generational Collection)算法, 这种算法会根据对象存活周期的不同将内存划分为几块, 如 JVM 中的新生代、老年代、永久代， 这样就可以根据各年代特点分别采用最适当的 GC 算法


### 11、描述一下什么情况下，对象会从年轻代进入老年代
### 12、什么是栈
### 13、CMS分为哪几个阶段?
### 14、safepoint是什么？
### 15、ZGC收集器中的染色指针有什么用？
### 16、说说类加载的过程
### 17、描述一下 JVM 加载 class 文件的原理机制
### 18、栈帧里面包含哪些东西？
### 19、谈谈对 OOM 的认识
### 20、GC 是什么？为什么要有 GC？
### 21、JRE、JDK、JVM 及 JIT 之间有什么不同？
### 22、JVM 如何确定垃圾对象？
### 23、堆和栈的区别
### 24、方法区
### 25、分代回收
### 26、JVM调优命令有哪些？
### 27、类加载为什么要使用双亲委派模式，有没有什么场景是打破了这个模式？
### 28、Minor GC与Full GC分别在什么时候发生？
### 29、说说ZGC垃圾收集器的工作原理
### 30、内存溢出和内存泄漏的区别？




## 全部答案，整理好了，直接下载吧

### 下载链接：[全部答案，整理好了](https://www.souyunku.com/wp-content/uploads/weixin/githup-weixin-2.png)




## 最新，高清PDF：172份，7701页，最新整理

[![大厂面试题](https://www.souyunku.com/wp-content/uploads/weixin/mst.png "架构师专栏")](https://www.souyunku.com/wp-content/uploads/weixin/githup-weixin.png "架构师专栏")

[![大厂面试题](https://www.souyunku.com/wp-content/uploads/weixin/githup-weixin.png "架构师专栏")](https://www.souyunku.com/wp-content/uploads/weixin/githup-weixin.png "架构师专栏")
