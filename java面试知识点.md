

# JVM

JVM 是可运行 Java 代码的虚拟机 ，包括字节码指令集、一组寄存器、栈、 一个垃圾回收，堆 和 一个存储方法域。JVM 是运行在操作系统之上的，它与硬件没有直接 的交互。

![image-20191219093717026](/Users/dylan/Library/Application Support/typora-user-images/image-20191219093717026.png)

Java 为什么能够 跨平台的原因:每一种平台的解释器是不同的，但是实现的虚拟机是相同的，当一个程序从开始运行，这时虚拟机就开始实例化了，多个程序启动就会 存在多个虚拟机实例。程序退出或者关闭，则虚拟机实例消亡，多个虚拟机实例之间数据不能共享。

![image-20191219093925659](/Users/dylan/Library/Application Support/typora-user-images/image-20191219093925659.png)

## 线程

![image-20191219094049259](/Users/dylan/Library/Application Support/typora-user-images/image-20191219094049259.png)

## jvm内存区域

JVM 内存区域主要分为线程私有区域【程序计数器、虚拟机栈、本地方法区】、线程共享区 域【JAVA 堆、方法区】、直接内存:

- 线程私有数据区域生命周期与线程相同, 依赖用户线程的启动/结束 而 创建/销毁(在 Hotspot VM 内, 每个线程都与操作系统的本地线程直接映射, 因此这部分内存区域的存/否跟随本地线程的 生/死对应)。
- 线程共享区域随虚拟机的启动/关闭而创建/销毁。
- 直接内存并不是 JVM 运行时数据区的一部分, 但也会被频繁的使用: 在 JDK 1.4 引入的 NIO 提 供了基于 Channel 与 Buffer 的 IO 方式, 它可以使用 Native 函数库直接分配堆外内存, 然后使用 DirectByteBuffer 对象作为这块内存的引用进行操作(详见: Java I/O 扩展), 这样就避免了在 Java 堆和 Native 堆中来回复制数据, 因此在一些场景中可以显著提高性能。

![image-20191219094358224](/Users/dylan/Library/Application Support/typora-user-images/image-20191219094358224.png)

1. 程序计数器

   每条线程都要有一个独立的程序计数器,正在执行 java 方法的话，计数器记录的是虚拟机字节码指令的地址(当前指令的地址)。如 果还是 Native 方法，则为空。这个内存区域是唯一一个在虚拟机中没有规定任何OutOfMemoryError 情况的区域。

2. 虚拟机栈**(**线程私有**)**

   是描述 java 方法执行的内存模型，每个方法在执行的同时都会创建一个栈帧(Stack Frame) 用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行完成 的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。

   栈帧( Frame)是用来存储数据和部分过程结果的数据结构，同时也被用来处理动态链接 (Dynamic Linking)、 方法返回值和异常分派( Dispatch Exception)。栈帧随着方法调用而创建，随着方法结束而销毁——无论方法是正常完成还是异常完成(抛出了在方法内未被捕获的异 常)都算作方法结束。

   <img src="/Users/dylan/Library/Application Support/typora-user-images/image-20191219094754255.png" alt="image-20191219094754255" style="zoom:50%;" />

**2.2.3.** 本地方法区**(**线程私有**)**

本地方法区和 Java Stack 作用类似, 区别是虚拟机栈为执行 Java 方法服务, 而本地方法栈则为 Native 方法服务, 如果一个 VM 实现使用 C-linkage 模型来支持 Native 调用, 那么该栈将会是一个 C 栈，但 HotSpot VM 直接就把本地方法栈和虚拟机栈合二为一。

**2.2.4.** 堆(**Heap-**线程共享)**-**运行时数据区

是被线程共享的一块内存区域，创建的对象和数组都保存在 Java 堆内存中，也是垃圾收集器进行 垃圾收集的最重要的内存区域。由于现代 VM 采用**分代收集算法**, 因此 Java 堆从 GC 的角度还可以 细分为: **新生代**(Eden 区、From Survivor 区和 To Survivor 区)和**老年代。**

**2.2.5.** 方法区**/**永久代(线程共享)

**永久代**(Permanent Generation)**, 用于存储**被 **JVM** **加载的类信息**、**常量**、**静 态变量**、**即时编译器编译后的代码**等数据. HotSpot VM 把 GC 分代收集扩展至方法区, 即**使用** **Java** **堆的永久代来实现方法区**, 这样 HotSpot 的垃圾收集器就可以像管理 Java 堆一样管理这部分内存, 而不必为方法区开发专门的内存管理器(永久带的内存回收的主要目标是针对**常量池的回收**和**类型 的卸载**, 因此收益一般很小)。

运行时常量池(Runtime Constant Pool)是方法区的一部分。Class 文件中除了有类的版 本、字段、方法、接口等描述等信息外，还有一项信息是常量池(Constant Pool Table)，用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加 载后存放到方法区的运行时常量池中。 Java 虚拟机对 Class 文件的每一部分(自然也包括常量 池)的格式都有严格的规定，每一个字节用于存储哪种数据都必须符合规范上的要求，这样才会 被虚拟机认可、装载和执行。

## 运行时内存
 Java 堆从 GC 的角度还可以细分为: **新生代**(Eden 区、From Survivor 区和 To Survivor 区)和**老年**代。

![image-20191219095410934](/Users/dylan/Library/Application Support/typora-user-images/image-20191219095410934.png)

#### 新生代
 是用来存放新生的对象。一般占据堆的 1/3 空间。由于频繁创建对象，所以新生代会频繁触发

MinorGC 进行垃圾回收。新生代又分为 Eden 区、ServivorFrom、ServivorTo 三个区。

- Eden 区:Java 新对象的出生地(如果新创建的对象占用内存很大，则直接分配到老 年代)。当 Eden 区内存不够的时候就会触发 MinorGC，对新生代区进行 一次垃圾回收。

-  ServivorFrom:上一次 GC 的幸存者，作为这一次 GC 的被扫描者。

- ServivorTo保留了一次 MinorGC 过程中的幸存者

MinorGC的过程(复制->清空->互换)

MinorGC 采用复制算法:

**1**. **eden**、**servicorFrom** 复制到 **ServicorTo**，年龄+1

首先，把 Eden 和 ServivorFrom 区域中存活的对象复制到 ServicorTo 区域(如果有对象的年 龄以及达到了老年的标准，则赋值到老年代区)，同时把这些对象的年龄+1(如果 ServicorTo 不 够位置了就放到老年区);

**2**. 清空 **eden**、**servicorFrom**

 然后，清空 Eden 和 ServicorFrom 中的对象;

**3**. **ServicorTo** 和 **ServicorFrom** 互换
 最后，ServicorTo 和 ServicorFrom 互换，原 ServicorTo 成为下一次 GC 时的 ServicorFrom区。

#### 老年代 

主要存放应用程序中生命周期长的内存对象。

老年代的对象比较稳定，所以 MajorGC 不会频繁执行。在进行 MajorGC 前一般都先进行 了一次 MinorGC，使得有新生代的对象晋身入老年代，导致空间不够用时才触发。当无法找到足够大的连续空间分配给新创建的较大对象时也会提前触发一次 MajorGC 进行垃圾回收腾出空间。

MajorGC 采用标记清除算法:首先扫描一次所有老年代，标记出存活的对象，然后回收没有标记的对象。MajorGC 的耗时比较长，因为要扫描再回收。MajorGC 会产生内存碎片，为了减少内存损耗，我们一般需要进行合并或者标记出来方便下次直接分配。当老年代也满了装不下的 时候，就会抛出 OOM(Out of Memory)异常。

#### 永久代

指内存的永久保存区域，主要存放 Class 和 Meta(元数据)的信息,Class 在被加载的时候被 放入永久区域，它和存放实例的区域不同,GC 不会在主程序运行期对永久区域进行清理。所以这也导致了永久代的区域会随着加载的 Class 的增多而胀满，最终抛出 OOM 异常。

 JAVA8 **与元数据**

在 Java8 中，永久代已经被移除，被一个称为“元数据区”(元空间)的区域所取代。元空间 的本质和永久代类似，元空间与永久代之间最大的区别在于:**元空间并不在虚拟机中，而是使用 本地内存**。因此，默认情况下，元空间的大小仅受本地内存限制。类的元数据放入 native memory, 字符串池和类的静态变量放入 java 堆中，这样可以加载多少类的元数据就不再由 MaxPermSize 控制, 而由系统的实际可用空间来控制。

## 垃圾回收与算法

如何确定垃圾:引用计数,可达性分析

算法:标记清除算法,复制算法,标记整理算法,分代收集算法(新生代复制,老年代标记复制)
		<img src="/Users/dylan/Library/Application Support/typora-user-images/image-20191219131103761.png" alt="image-20191219131103761" style="zoom:67%;" />

## 四种引用类型

强引用:把一个对象赋给一个引用变量，这个引用变量就是一个强引 用。当一个对象被强引用变量引用时

软引用:软引用需要用 SoftReference 类来实现

弱引用WeakReference 

虚引用:它不能单独使用，必须和引用队列联合使用。虚 引用的主要作用是跟踪对象被垃圾回收的状态。

## 分代收集算法&分区收集

## 垃圾收集器

**Serial** 垃圾收集器(单线程、复制算法)

 **ParNew**垃圾收集器(**Serial+**多线程)

**Parallel Scavenge** 收集器(多线程复制算法、高效)

**SerialOld**收集器(单线程标记整理算法),Serial Old 是 Serial 垃圾收集器年老代版本,行在 Client 默认的 java 虚拟机默认的年老代垃圾收集器。

**ParallelOld**收集器(多线程标记整理算法)

**CMS**收集器(多线程标记清除算法):初始标记,并发标记,重新标记,并发清除

<img src="/Users/dylan/Library/Application Support/typora-user-images/image-20191219131910189.png" alt="image-20191219131910189" style="zoom:67%;" />

G1收集器

## java IO/NIO

阻塞 **IO** 模型,非阻塞 **IO** 模型,多路复用 **IO** 模型,信号驱动 **IO** 模型,异步 **IO** 模型

**2.8.1.** 阻塞 **IO** 模型

最传统的一种 IO 模型，即在读写数据过程中会发生阻塞现象。当用户线程发出 IO 请求之后，内 核会去查看数据是否就绪，如果没有就绪就会等待数据就绪，而用户线程就会处于阻塞状态，用 户线程交出 CPU。当数据就绪之后，内核会将数据拷贝到用户线程，并返回结果给用户线程，用户线程才解除 block 状态。典型的阻塞 IO 模型的例子为:`data = socket.read()`;如果数据没有就绪，就会一直阻塞在 read 方法。

**2.8.2.** 非阻塞 **IO** 模型(问一下问一下问一下,直到好了为止)

当用户线程发起一个 read 操作后，并不需要等待，而是马上就得到了一个结果。如果结果是一个 error 时，它就知道数据还没有准备好，于是它可以再次发送 read 操作。一旦内核中的数据准备 好了，并且又再次收到了用户线程的请求，那么它马上就将数据拷贝到了用户线程，然后返回。 所以事实上，在非阻塞 IO 模型中，用户线程需要不断地询问内核数据是否就绪，也就说非阻塞 IO 不会交出 CPU，而会一直占用 CPU。典型的非阻塞 IO 模型一般如下:

```java
while(true){
	data = socket.read();
	if(data != error){
		//处理数据
		break;
	}
}
```

但是对于非阻塞 IO 就有一个非常严重的问题，在 while 循环中需要不断地去询问内核数据是否就绪，这样会导致 CPU 占用率非常高，因此一般情况下很少使用 while 循环这种方式来读取数据。

**2.8.3.** 多路复用 **IO** 模型(一个线程负责管理)

多路复用 IO 模型是目前使用得比较多的模型。Java NIO 实际上就是多路复用 IO。

在多路复用 IO 模型中，会有一个线程不断去轮询多个 socket 的状态，只有当 socket 真正有读写事件时，才真正调用实际的 IO 读写操作。因为在多路复用 IO 模型中，只需要使用一个线程就可以管理多个 socket，系统不需要建立新的进程或者线程，也不必维护这些线程和进程，并且只有在真正有 socket 读写事件进行时，才会使用 IO 资源，所以它大大减少了资源占用。在 Java NIO 中，是通 过 **selector.select()**去查询每个通道是否有到达事件，如果没有事件，则一直阻塞在那里，因此这 种方式会导致用户线程的阻塞。多路复用 IO 模式，通过一个线程就可以管理多个 socket，只有当 socket 真正有读写事件发生才会占用资源来进行实际的读写操作。因此，多路复用 IO 比较适合连 接数比较多的情况。

另外多路复用 IO 为何比非阻塞 IO 模型的效率高?
是因为在非阻塞 IO 中，不断地询问 socket 状态时通过**用户线程**去进行的，而在多路复用 IO 中，轮询每个 socket 状态是**内核**在进行的，这个效率要比用户线程要高的多。

不过要注意的是，多路复用 IO 模型是通过轮询的方式来检测是否有事件到达，并且对到达的事件 逐一进行响应。因此对于多路复用 IO 模型来说，一旦事件响应体很大，那么就会导致后续的事件 迟迟得不到处理，并且会影响新的事件轮询。

**2.8.4.** 信号驱动 **IO** 模型

在信号驱动 IO 模型中，当用户线程发起一个 IO 请求操作，会给对应的 socket 注册一个信号函 数，然后用户线程会继续执行，当内核数据就绪时会发送一个信号给用户线程，用户线程接收到 信号之后，便在信号函数中调用 IO 读写操作来进行实际的 IO 请求操作。

**2.8.5.** 异步 **IO** 模型

异步 IO 模型才是最理想的 IO 模型，在异步 IO 模型中，当用户线程发起 read 操作之后，立刻就 可以开始去做其它的事。而另一方面，从内核的角度，当它受到一个 asynchronous read 之后， 它会立刻返回，说明 read 请求已经成功发起了，因此不会对用户线程产生任何 block。然后，内核会等待数据准备完成，然后将数据拷贝到用户线程，当这一切都完成之后，内核会给用户线程 发送一个信号，告诉它 read 操作完成了。也就说用户线程完全不需要实际的整个 IO 操作是如何 进行的，只需要先发起一个请求，当接收内核返回的成功信号时表示 IO 操作已经完成，可以直接 去使用数据了。也就说在异步 IO 模型中，IO 操作的两个阶段都不会阻塞用户线程，这两个阶段都是由内核自动完 成，然后发送一个信号告知用户线程操作已完成。用户线程中不需要再次调用 IO 函数进行具体的 读写。这点是和信号驱动模型有所不同的，在信号驱动模型中，当用户线程接收到信号表示数据 已经就绪，然后需要用户线程调用 IO 函数进行实际的读写操作;而在异步 IO 模型中，收到信号 表示 IO 操作已经完成，不需要再在用户线程中调用 IO 函数进行实际的读写操作。 

### java IO包

![image-20191219134601423](/Users/dylan/Library/Application Support/typora-user-images/image-20191219134601423.png)

### java NIO

NIO 主要有三大核心部分:Channel(通道)，Buffer(缓冲区), Selector。

传统 IO 基于字节流和字符流进行操作，而 NIO 基于 Channel 和 Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。Selector(选择区)用于监听多个通道的事件(比如:连接打开， 数据到达)。因此，单个线程可以监听多个数据通道。

#### NIO **的缓冲区**

Java IO 面向流意味着每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何 地方。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据，需要先将它缓 存到一个缓冲区。NIO 的缓冲导向方法不同。数据读取到一个它稍后处理的缓冲区，需要时可在 缓冲区中前后移动。这就增加了处理过程中的灵活性。但是，还需要检查是否该缓冲区中包含所 有您需要处理的数据。而且，需确保当更多的数据读入缓冲区时，不要覆盖缓冲区里尚未处理的 数据。

#### NIO **的非阻塞**

IO 的各种流是阻塞的。这意味着，当一个线程调用 read() 或 write()时，该线程被阻塞，直到有 一些数据被读取，或数据完全写入。该线程在此期间不能再干任何事情了。 NIO 的非阻塞模式， 使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可 用时，就什么都不会获取。而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以 继续做其他的事情。 非阻塞写也是如此。一个线程请求写入一些数据到某通道，但不需要等待它 完全写入，这个线程同时可以去做别的事情。 线程通常将非阻塞 IO 的空闲时间用于在其它通道上 							 	

执行 IO 操作，所以一个单独的线程现在可以管理多个输入和输出通道(channel)。 							 					 

### channel

首先说一下 Channel，国内大多翻译成“通道”。Channel 和 IO 中的 Stream(流)是差不多一个 等级的。只不过 Stream 是单向的，譬如:InputStream, OutputStream，而 Channel 是双向 的，既可以用来进行读操作，又可以用来进行写操作。
 NIO 中的 Channel 的主要实现有:

1. FileChannel					(文件)

2. DatagramChannel         (UDP)

3. SocketChannel               (TCP)
4. ServerSocketChannel。(TCP) 

这里看名字就可以猜出个所以然来:分别可以对应文件 IO、UDP 和 TCP(Server 和 Client)。 下面演示的案例基本上就是围绕这 4 个类型的 Channel 进行陈述的。

### Buffer

Buffer，故名思意，缓冲区，实际上是一个容器，是一个连续数组。Channel 提供从文件、 网络读取数据的渠道，但是读取或写入的数据都必须经由 Buffer。

![image-20191219141432202](/Users/dylan/Library/Application Support/typora-user-images/image-20191219141432202.png)

上面的图描述了从一个客户端向服务端发送数据，然后服务端接收数据的过程。客户端发送 数据时，必须先将数据存入 Buffer 中，然后将 Buffer 中的内容写入通道。服务端这边接收数据必 须通过 Channel 将数据读入到 Buffer 中，然后再从 Buffer 中取出数据来处理。

在 NIO 中，Buffer 是一个顶层父类，它是一个抽象类，常用的 Buffer 的子类有: ByteBuffer、IntBuffer、 CharBuffer、 LongBuffer、 DoubleBuffer、FloatBuffer、 ShortBuffer

### Selector

Selector 类是 NIO 的核心类，Selector 能够检测多个注册的通道上是否有事件发生，如果有事 件发生，便获取事件然后针对每个事件进行相应的响应处理。这样一来，只是用一个单线程就可 以管理多个通道，也就是管理多个连接。这样使得只有在连接真正有读写事件发生时，才会调用 函数来进行读写，就大大地减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护 多个线程，并且避免了多线程之间的上下文切换导致的开销。

## JVM类加载机制

**加载**:这个阶段会在内存中生成一个代表这个类的 java.lang.Class 对 象，作为方法区这个类的各种数据的入口

**验证**:确保 Class 文件的字节流中包含的信息是否符合当前虚拟机的要求

**准备** :在方法区中分配这些变量所使 用的内存空间,默认值为0之类的,除了 static final

**解析**: 常量池中:符号引用 => 直接引用

**初始化**:初始化阶段是执行类构造器< client>方法的过程。< client>方法是由编译器自动收集类中的类变 量的赋值操作和静态语句块中的语句合并而成的。虚拟机会保证子< client>方法执行之前，父类 的< client>方法已经执行完毕，如果一个类中没有对静态变量赋值也没有静态语句块，那么编译 器可以不为这个类生成< client>()方法。

## 类加载器

![image-20191219142830405](/Users/dylan/Library/Application Support/typora-user-images/image-20191219142830405.png)

### 双亲委派

当一个类收到了类加载请求，他首先不会尝试自己去加载这个类，而是把这个请求委派给父类去完成，每一个层次类加载器都是如此，因此所有的加载请求都应该传送到启动类加载其中，只有当父类加载器反馈自己无法完成这个请求的时候(在它的加载路径下没有找到所需加载的Class)，子类加载器才会尝试自己去加载。

采用双亲委派的一个好处是比如加载位于 rt.jar 包中的类 java.lang.Object，不管是哪个加载器加载这个类，最终都是委托给顶层的启动类加载器进行加载，这样就保证了使用不同的类加载器最终得到的都是同样一个 Object 对象。![image-20191219142932726](/Users/dylan/Library/Application Support/typora-user-images/image-20191219142932726.png)

## 动态模型系统

OSGi(Open Service Gateway Initiative)，是面向 Java 的动态模型系统，是 Java 动态化模块化系 统的一系列规范。

****

**动态改变构造**

OSGi 服务平台提供在多种网络设备上无需重启的动态改变构造的功能。为了最小化耦合度和促使 这些耦合度可管理，OSGi 技术提供一种面向服务的架构，它能使这些组件动态地发现对方。

**模块化编程与热插拔**

OSGi 旨在为实现 Java 程序的模块化编程提供基础条件，基于 OSGi 的程序很可能可以实现模块级 的热插拔功能，当程序升级更新时，可以只停用、重新安装然后启动程序的其中一部分，这对企 业级程序开发来说是非常具有诱惑力的特性。

OSGi 描绘了一个很美好的模块化开发目标，而且定义了实现这个目标的所需要服务与架构，同时 也有成熟的框架进行实现支持。但并非所有的应用都适合采用 OSGi 作为基础架构，它在提供强大 功能同时，也引入了额外的复杂度，因为它不遵守了类加载的双亲委托模型。



# JAVA集合

## 接口继承关系和实现

set(集)、list(列表包含 Queue)和 map(映射)。

1. Collection:Collection 是集合 List、Set、Queue 的最基本的接口。
2. Iterator:迭代器，可以通过迭代器遍历集合中的数据
3. Map:是映射表的基础接口

![image-20191219143250891](/Users/dylan/Library/Application Support/typora-user-images/image-20191219143250891.png)



![image-20191219143406813](/Users/dylan/Library/Application Support/typora-user-images/image-20191219143406813.png)

## List

**1. ArrayList**(数组)

ArrayList 是最常用的 List 实现类，内部是通过数组实现的，它允许对元素进行快速随机访问。数

组的缺点是每个元素之间不能有间隔，当数组大小不满足时需要增加存储能力，就要将已经有数

组的数据复制到新的存储空间中。当从 ArrayList 的中间位置插入或者删除元素时，需要对数组进

行复制、移动、代价比较高。因此，它适合随机查找和遍历，不适合插入和删除。

**2. Vector**(数组实现、线程同步)

Vector 与 ArrayList 一样，也是通过数组实现的，不同的是它支持线程的同步，即某一时刻只有一

个线程能够写 Vector，避免多线程同时写而引起的不一致性，但实现同步需要很高的花费，因此，

访问它比访问 ArrayList 慢。

**3. LinkList**(链表)

LinkedList 是用链表结构存储数据的，很适合数据的动态插入和删除，随机访问和遍历速度比较 慢。另外，他还提供了 List 接口中没有定义的方法，专门用于操作表头和表尾元素，可以**当作堆 栈、队列和双向队列使用。**

## Set

**1. HashSet**(**Hash** 表)

 HashSet 首先判断两个元素的哈希值，如果哈希值一样，接着会看 equals 方法 如果 equals ,HashSet 就视为同一个元素。

**2.**TreeSet(二叉树)

1. TreeSet()是使用二叉树的原理对新 add()的对象按照指定的顺序排序(升序、降序)，每增 加一个对象都会进行排序，将对象插入的二叉树指定的位置。

2. Integer 和 String 对象都可以进行默认的 TreeSet 排序，而自定义类的对象是不可以的，自 己定义的类必须实现 Comparable 接口，并且覆写相应的 compareTo()函数，才可以正常使 用。

   在覆写 compare()函数时，要返回相应的值才能使 TreeSet 按照一定的规则来排序

   比较此对象与指定对象的顺序。如果该对象小于、等于或大于指定对象，则分别返回负整

   数、零或正整数。

**3.LinkedHashSet**(**HashSet+LinkedHashMap**)

`LinkedHashSet extends HashSet implements LinkedHashMap`

LinkedHashSet 底层使用 LinkedHashMap 来保存所有元素，它继承与 HashSet，其所有的方法

操作上又与 HashSet 相同，因此 LinkedHashSet 的实现上非常简单，只提供了四个构造方法，并

通过传递一个标识参数，调用父类的构造器，底层构造一个 LinkedHashMap 来实现，在相关操

作上与父类 HashSet 的操作相同，直接调用父类 HashSet 的方法即可。

**HashMap的java8实现**

Java8 对 HashMap 进行了一些修改，最大的不同就是利用了红黑树，所以其由 数组+链表+红黑

树 组成。

根据 Java7 HashMap 的介绍，我们知道，查找的时候，根据 hash 值我们能够快速定位到数组的

具体下标，但是之后的话，需要顺着链表一个个比较下去才能找到我们需要的，时间复杂度取决

于链表的长度，为 O(n)。为了降低这部分的开销，在 Java8 中，当链表中的元素超过了 8 个以后，

会将链表转换为红黑树，在这些位置进行查找的时候可以降低时间复杂度为 O(logN)。

## Map

### HashMap(数组**+**链表**+**红黑树)

HashMap 根据键的 hashCode 值存储数据，大多数情况下可以直接定位到它的值，因而具有很快

的访问速度，但遍历顺序却是不确定的。 HashMap 最多只允许一条记录的键为 null，允许多条记

录的值为 null。HashMap 非线程安全，即任一时刻可以有多个线程同时写 HashMap，可能会导

致数据的不一致。如果需要满足线程安全，可以用 Collections 的 synchronizedMap 方法使

HashMap 具有线程安全的能力，或者使用 ConcurrentHashMap。

jdk7中是链表

jdk8中是红黑树

### ConcurrentHashMap

**Segment** 段

ConcurrentHashMap 和 HashMap 思路是差不多的，但是因为它支持并发操作，所以要复杂一 些。整个 ConcurrentHashMap 由一个个 Segment 组成，Segment 代表”部分“或”一段“的 意思，所以很多地方都会将其描述为分段锁。注意，行文中，我很多地方用了“槽”来代表一个 segment。

**线程安全(Segment 继承 ReentrantLock 加锁)**

简单理解就是，ConcurrentHashMap 是一个 Segment 数组，Segment 通过继承 ReentrantLock 来进行加锁，所以每次需要加锁的操作锁住的是一个 segment，这样只要保证每 个 Segment 是线程安全的，也就实现了全局的线程安全。

![image-20191219145216711](/Users/dylan/Library/Application Support/typora-user-images/image-20191219145216711.png)

**3.**并行度(默认 **16**)

concurrencyLevel:并行级别、并发数、Segment 数，怎么翻译不重要，理解它。默认是 16， 也就是说 ConcurrentHashMap 有 16 个 Segments，所以理论上，这个时候，最多可以同时支 持 16 个线程并发写，只要它们的操作分别分布在不同的 Segment 上。这个值可以在初始化的时 候设置为其他值，但是一旦初始化以后，它是不可以扩容的。再具体到每个 Segment 内部，其实 每个 Segment 很像之前介绍的 HashMap，不过它要保证线程安全，所以处理起来要麻烦些。

**Java8** 实现 (引入了红黑树)

![image-20191219145336061](/Users/dylan/Library/Application Support/typora-user-images/image-20191219145336061.png)

### HashTable(线程安全)

Hashtable 是遗留类，很多映射的常用功能与 HashMap 类似，不同的是它承自 Dictionary 类，

并且是线程安全的，任一时间只有一个线程能写 Hashtable，并发性不如 ConcurrentHashMap，

因为 ConcurrentHashMap 引入了分段锁。Hashtable 不建议在新代码中使用，不需要线程安全

的场合可以用 HashMap 替换，需要线程安全的场合可以用 ConcurrentHashMap 替换。

### TreeMap(可排序)

TreeMap 实现 SortedMap 接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，

也可以指定排序的比较器，当用 Iterator 遍历 TreeMap 时，得到的记录是排过序的。

在使用 TreeMap 时，key 必须实现 Comparable 接口或者在构造 TreeMap 传入自定义的

Comparator，否则会在运行时抛出 java.lang.ClassCastException 类型的异常。

###  LinkHashMap(记录插入顺序)

inkedHashMap 是 HashMap 的一个子类，保存了记录的插入顺序，在用 Iterator 遍历

LinkedHashMap 时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序。

# Java多线程并发

## java并发知识库

<img src="/Users/dylan/Library/Application Support/typora-user-images/image-20191219145559176.png" alt="image-20191219145559176" style="zoom:200%;" />

## java线程实现/创建方式

### 继承thread类

Thread 类本质上是实现了 Runnable 接口的一个实例，代表一个线程的实例。启动线程的唯一方 法就是通过 Thread 类的 start()实例方法。start()方法是一个 native 方法，它将启动一个新线 程，并执行 run()方法。

```java
public class MyThread extends Thread {
	public void run() {
    System.out.println("MyThread.run()");
	}
}

MyThread myThread1 = new MyThread();
myThread1.start();
```

### 实现 **Runnable** 接口。

如果自己的类已经 extends 另一个类，就无法直接 extends Thread，此时，可以实现一个 Runnable 接口。

```java
public class MyThread extends OtherClass implements Runnable {
	public void run() {
		System.out.println("MyThread.run()");
	}
}
//启动 MyThread，需要首先实例化一个 Thread，并传入自己的 MyThread 实例:
 MyThread myThread = new MyThread();
 Thread thread = new Thread(myThread);
 thread.start();
 //事实上，当传入一个 Runnable target 参数给 Thread 后，Thread 的 run()方法就会调用
 target.run()
 public void run() {
	 if (target != null) {
     target.run();
  }
 }
```

### **ExecutorService**、**Callable**、**Future** 有返回值线程

有返回值的任务必须实现 Callable 接口，类似的，无返回值的任务必须 Runnable 接口。执行 Callable 任务后，可以获取一个 Future 的对象，在该对象上调用 get 就可以获取到 Callable 任务 返回的 Object 了，再结合线程池接口 ExecutorService 就可以实现传说中有返回结果的多线程 了。

### 四种线程池

# java基础

## java异常分类及处理

![image-20191219153909779](/Users/dylan/Library/Application Support/typora-user-images/image-20191219153909779.png)

处理:

三种:一是 throw,一个 throws，还有一种系统自动抛异常。

Throw,throws

- 位置:throws 用在函数上，后面跟的是异常类，可以跟多个;而 throw 用在函数内，后面跟的

  是异常对象

- throws 用来声明异常，让调用者只知道该功能可能出现的问题，可以给出预先的处理方

  式;

  throw 抛出具体的问题对象，执行到 throw，功能就已经结束了，跳转到调用者，并

  将具体的问题对象抛给调用者。也就是说 throw 语句独立存在时，下面不要定义其他语

  句，因为执行不到。

  throws 表示出现异常的一种可能性，并不一定会发生这些异常;throw 则是抛出了异常，

  执行 throw 则一定抛出了某种异常对象

- 两者都是消极处理异常的方式，只是抛出或者可能抛出异常，但是不会由函数去处理异常，真正的处理异常由函数的上层调用处理。

## 反射

### **反射机制概念** (运行状态中知道类所有的属性和方法)

在 Java 中的反射机制是指在运行状态中，对于任意一个类都能够知道这个类所有的属性和方法;

并且对于任意一个对象，都能够调用它的任意一个方法;这种动态获取信息以及动态调用对象方

法的功能成为 Java 语言的反射机制。

### 应用场合

Person p=new Student(); 其中编译时类型为 Person，运行时类型为 Student。

该对象的编译时类型为 Person,但是程序有需要调用该对象的运行时类型的方法。为了解决这些问题，程序需要在运行时发现对象和类的真实信息。嗷就是编译的时候认为它是person,但是运行的时候又需要知道Student的方法,反射就是在这个时候可以知道student的方法的

### API

- Class 类:反射的核心类，可以获取类的属性，方法等信息。

- Field 类:Java.lang.reflec 包中的类，表示类的**成员变量**，可以用来获取和设置类之中的属性值。

- Method 类: Java.lang.reflec 包中的类，表示**类的方法**，它可以用来获取类中的方法信息或者执行方法。

- Constructor 类: Java.lang.reflec 包中的类，表示类的**构造方法**。

### 使用步骤:

1. 获取class对象:

   - Person p=new Person();

     Class clazz=p.getClass();

   - Class clazz=Person.class;

   - Class clazz=Class.forName("类的全路径"); (最常用)

2. 调用对象方法

   ```java
   //获取 Person 类的所有方法信息
   Method[] method=clazz.getDeclaredMethods(); 
   for(Method m:method){
   	System.out.println(m.toString()); 
   }
   //获取 Person 类的所有成员属性信息
   Field[] field=clazz.getDeclaredFields(); 
   for(Field f:field){
   	System.out.println(f.toString()); 
   }
   //获取 Person 类的所有构造方法信息
   Constructor[] constructor=clazz.getDeclaredConstructors(); 
   for(Constructor c:constructor){
   	System.out.println(c.toString());
   }
   ```

   ### 创建对象的两种方法

   1. Class -> newInstance()
   2. Class -> constructor -> newInstance

   ```java
   //获取 Person 类的 Class 对象
   Class clazz=Class.forName("reflection.Person"); 
   //1. 使用.newInstane 方法创建对象
   Person p=(Person) clazz.newInstance();
   //2.获取构造方法并创建对象+创建对象并设置属性
    Constructor c=clazz.getDeclaredConstructor(String.class,String.class,int.class);
    Person p1=(Person) c.newInstance("李四","男",20);
   ```

   ## java注解

   ### 概念

   annotation(注解)是 Java 提供的一种对元程序中元素关联信息和元数据(metadata)的途径

   和方法。Annatation(注解)是一个接口，程序可以通过反射来获取指定程序中元素的 Annotation

   对象，然后通过该 Annotation 对象来获取注解中的元数据信息。

   ### 四种标准元注解

   元注解的作用是负责注解其他注解。 Java5.0 定义了 4 个标准的 meta-annotation 类型，它们被用来提供对其它 annotation 类型作说明。

   **@Target** 修饰的对象范围,Annotation 所修饰的对象范围: Annotation 可被用于 packages、types(类、接口、枚举、Annotation 类型)、类型成员(方法、构造方法、成员变量、枚举值)、方法参数和本地变量(如循环变量、catch 参数)。

   **@Retention** 定义 被保留的时间长短

   **@Documented** 描述**-javadoc**

   **@Inherited** 阐述了某个被标注的类型是被继承的

   ![image-20191219161002917](/Users/dylan/Library/Application Support/typora-user-images/image-20191219161002917.png)



## java内部类

静态内部类(静态的内部类)+成员内部类(非静态的内部类)+局部内部类(方法中的类,只用于一个方法的类)+匿名内部类

### 静态内部类

定义在类内部的静态类,就是静态内部类

```java
public class Out {
	private static int a; 
  private int b;
	public static class Inner {
		public void print() {
      System.out.println(a);
		} 
  }
}
```

1. 静态内部类可以访问外部类所有的静态变量和方法，即使是 private 的也一样。

2. 静态内部类和一般类一致，可以定义静态变量、方法，构造方法等。

3. 其它类使用静态内部类需要使用“外部类.静态内部类”方式

   ```java
   Out.Inner inner =new Out.Inner();
   inner.print();
   ```

4. Java 集合类 HashMap 内部就有一个静态内部类 Entry。Entry 是 HashMap 存放元素的抽象，HashMap 内部维护 Entry 数组用了存放元素，但是 Entry 对使用者是透明的。像这种和外部类关系密切的，且不依赖外部类实例的，都可以使用静态内部类。

### 成员内部类

定义在类内部的非静态类，就是成员内部类。成员内部类不能定义静态方法和变量(final 修饰的除外)。这是因为成员内部类是非静态的，类初始化的时候先初始化静态成员，如果允许成员内部类定义静态变量，那么成员内部类的静态变量初始化顺序是有歧义的。

### 匿名内部类

匿名内部类(要继承一个父类或者实现一个接口、直接使用 **new** 来生成一个对象的引用),没有class关键字

```java
public abstract class Bird { private String name;
	public String getName() { 
    return name;
	}
	public void setName(String name) {
		this.name = name; 
  }
	public abstract int fly();
}
public class Test {
	public void test(Bird bird){
		System.out.println(bird.getName() + "能够飞 " + bird.fly() + "米"); 
  }
	public static void main(String[] args) { 
    Test test = new Test();
		test.test(new Bird() {
			public int fly() { 
      	return 10000;
			}
			public String getName() {
				return "大雁"; 
    	}
		});
	}
}
```

把上面的匿名内部类提取出来:

```java
//原来是这样子的:
test.test(bird);

//把bird替换成了这些:
new Bird() {
		public int fly() { 
      return 10000;
		}
		public String getName() {
			return "大雁"; 
		}
}
```

## java 泛型

```java
// 泛型方法 printArray
public static < E > void printArray( E[] inputArray ) {
	for ( E element : inputArray ){ 
    System.out.printf( "%s ", element );
	}
}
/*
1. <? extends T>表示该通配符所代表的类型是 T 类型的子类。
2. <? super T>表示该通配符所代表的类型是 T 类型的父类。
*/

```

```java
//泛型类
public class Box<T> {
 private T t;
 public void add(T t) {
 		this.t = t;
 }
  public T get() {
 		return t;
	}
}
```

**类型通配符:**

类型通配符一般是使用?代替具体的类型参数。例如 List<?> 在逻辑上是List< String>,List< Integer> 等所有 List<具体类型实参>的父类。

**类型擦除**

Java 中的泛型基本上都是在编译器这个层次来实现的。在生成的 Java 字节代码中是不包含泛

型中的类型信息的。使用泛型的时候加上的类型参数，会被编译器在编译的时候去掉。这个

过程就称为类型擦除。如在代码中定义的 List<Object>和 List<String>等类型，在编译之后

都会变成 List。JVM 看到的只是 List，而由泛型附加的类型信息对 JVM 来说是不可见的。

类型擦除的基本过程也比较简单，首先是找到用来替换类型参数的具体类。这个具体类一般

是 Object。如果指定了类型参数的上界的话，则使用这个上界。把代码中的类型参数都替换

成具体的类。

## java序列化(创建可复用的java对象)

**保存(持久化)对象及其状态到内存或者磁盘**

Java 平台允许我们在内存中创建可复用的 Java 对象，但一般情况下，只有当 JVM 处于运行时，这些对象才可能存在，即，这些对象的生命周期不会比 JVM 的生命周期更长。但在现实应用中，就可能要求在 JVM 停止运行之后能够保存(持久化)指定的对象，并在将来重新读取被保存的对象。Java 对象序列化就能够帮助我们实现该功能。

**序列化对象以字节数组保持-静态成员不保存**

使用 Java 对象序列化，在保存对象时，会把其状态保存为一组字节，在未来，再将这些字节组装成对象。必须注意地是，对象序列化保存的是对象的”状态”，即它的成员变量。由此可知，对象序列化不会关注类中的静态变量。

**序列化用户远程对象传输**

除了在持久化对象时会用到对象序列化之外，当使用 RMI(远程方法调用)，或在网络中传递对象时，

都会用到对象序列化。Java 序列化 API 为处理对象序列化提供了一个标准机制，该 API 简单易用。

**Serializable 实现序列化**
在 Java 中，只要一个类实现了 java.io.Serializable 接口，那么它就可以被序列化。
ObjectOutputStream 和 ObjectInputStream 对对象进行序列化及反序列化

**writeObject** 和 **readObject** 自定义序列化策略
 在类中增加 writeObject 和 readObject 方法可以实现自定义序列化策略。

**序列化 ID**

虚拟机是否允许反序列化，不仅取决于类路径和功能代码是否一致，一个非常重要的一点是两个类的序列化 ID 是否一致(就是 private static final long serialVersionUID)

**序列化并不保存静态变量**
序列化子父类说明:要想将父类对象也序列化，就需要让父类也实现 Serializable 接口。

**Transient** 关键字阻止该变量被序列化到文件中 

在变量声明前加上Transient 关键字，可以阻止该变量被序列化到文件中，在被反序列化后，transient 变量的值被设为初始值，如 int 型的是 0，对象型的是 null。

应用场景:服务器端给客户端发送序列化对象数据，对象中有一些数据是敏感的，比如密码字符串等，希望对该密码字段在序列化时，进行加密，而客户端如果拥有解密的密钥，只有在客户端进行反序列化时，才可以对密码进行读取，这样可以一定程度保证序列化对象的数据安全。

## java复制

将一个对象的引用复制给另外一个对象，一共有三种方式:直接赋值,浅拷贝,深拷贝。

1. 直接赋值:`A a1 = a2`

2. **浅拷贝**:   值类型复制;引用类型则复制引用但不复制引用的对象

   ```java
   class Resume implements Cloneable{
   	public Object clone() {
   	 try {
   		 return (Resume)super.clone();
   	 } catch (Exception e) {
        e.printStackTrace();
    	}
   	return null;
   }
   ```

3. **深复制(复制对象和其应用对象)**

   ```java
   class Student implements Cloneable { String name;
   	int age;
   	Professor p;
   	Student(String name, int age, Professor p) { 
       this.name = name;
   		this.age = age;
   		this.p = p;
   	}
   	public Object clone() {
   		Student o = null; 
       try {
   			o = (Student) super.clone();
   		} catch (CloneNotSupportedException e) {
   			System.out.println(e.toString()); 
       }
   		o.p = (Professor) p.clone();//只要加上这个就可以
   		return o; 
     }
   }
   ```

4. 序列化:先实现serializable,把对象变成流,再从流中读出来

# Spring原理

## 特点:

![image-20191219165300027](/Users/dylan/Library/Application Support/typora-user-images/image-20191219165300027.png)

### 核心组件

![image-20191219165319983](/Users/dylan/Library/Application Support/typora-user-images/image-20191219165319983.png)

### 常用模块

核心容器:BeanFactory,使用IOC,讲应用程序的配置和依赖性规范与实际的应用程序代码分开

上下文,AOP,DAO,ORM,WEB,MVC

### 主要包

- org.springframework.core,核心工具包
- org.springframework.beans,访问配置文件,创建管理bean
- org.springframework.aop,提供aop
- org.springframework.context 基础IOC的扩展
- org.springframework.web.mvc SpringMVC的核心类
- org.springframework.transaction 事务
- org.springframework.web 
- org.springframework.aspects 
- org.springframework.test 对unit测试
- org.springframework.jdbc
- org.springframework.orm

### 常用注解

@Controller 控制层组件

@RestController =  Controller + responseBody

@Component: 泛指组件

@Service: 业务层

@ResponseBody:异步请求,将Controller的方法返回的对象,通过HttpMessageConverter转换为指定格式之后,写入到Response对象的body数据区域

@RequestMapping : 用来请求地址映射的直接

@AutoWired 完成自动装配

@PathVariable 取出URL模版中的变量作为参数

@requestParam SpringMVC后台控制层获取参数

@RequestHeader Request请求header部分的值绑定到参数的方法上

### 第三方结合

1. 权限:shiro 
2. 缓存: Ehcache(进程内缓存框架),redis
3. 持久层框架:Hibernate(???),Mybatis
4. 定时任务:quartz,spring-taxk
5. 校验框架:hibernate validator,oval

### IOC原理

原理:(背下来)

Spring 启动时读取应用程序提供的 Bean 配置信息，并在 Spring 容器中生成一份相应的 Bean 配 置注册表，然后根据这张注册表实例化 Bean，装配好 Bean 之间的依赖关系，为上层应用提供准 备就绪的运行环境。其中 Bean 缓存池为 HashMap 实现

![image-20191219173352478](/Users/dylan/Library/Application Support/typora-user-images/image-20191219173352478.png)

#### 框架基础设施

![image-20191219175405476](/Users/dylan/Library/Application Support/typora-user-images/image-20191219175405476.png)

***BeanDefinitionRegistry* 注册表**

Spring 配置文件中每一个节点元素在 Spring 容器里都通过一个 BeanDefinition 对象表示， 它描述了 Bean 的配置信息。而 BeanDefinitionRegistry 接口提供了向容器手工注册 BeanDefinition 对象的方法。

***BeanFactory* 顶层接口**

位于类结构树的顶端 ，它最主要的方法就是 getBean(String beanName)，该方法从容器中 返回特定名称的 Bean，BeanFactory 的功能通过其他的接口得到不断扩展:

***ListableBeanFactory***

该接口定义了访问容器中 Bean 基本信息的若干方法，如查看 Bean 的个数、获取某一类型 Bean 的配置名、查看容器中是否包括某一 Bean 等方法;

***HierarchicalBeanFactory* 父子级联**

父子级联 IoC 容器的接口， Spring 的 IoC 容器可以建立父子层级关联的容器体系，子容器可以通过接口方法访问父容器; 但父容器不能访问子容器的 Bean。

Spring 使用父子容器实 现了很多功能，比如在 Spring MVC 中，展现层 Bean 位于一个子容器中，而业务层和持久 层的 Bean 位于父容器中。这样，展现层 Bean 就可以引用业务层和持久层的 Bean，而业务层和持久层的 Bean 则看不到展现层的 Bean。

***ConfigurableBeanFactory***

是一个重要的接口，增强了 IoC 容器的可定制性，它定义了设置类装载器、属性编辑器、容器初始化后置处理器等方法;

***AutowireCapableBeanFactory* 自动装配**
定义了将容器中的 Bean 按某种规则(如按名字匹配、按类型匹配等)进行自动装配的方法;

 ***SingletonBeanRegistry*** 运行期间注册单例 *Bean*

定义了允许在运行期间向容器注册单实例 Bean 的方法;

对于单实例( singleton)的 Bean 来说，BeanFactory 会缓存 Bean 实例，所以第二次使用 getBean() 获取 Bean 时将直接从 IoC 容器的缓存中获取 Bean 实例。

Spring 在 DefaultSingletonBeanRegistry 类中提供了一 个用于缓存单实例 Bean 的缓存器，它是一个用 HashMap 实现的缓存器，单实例的 Bean 以 beanName 为键保存在这个 HashMap 中。

**依赖日志框框**
 在初始化 BeanFactory 时，必须为其提供一种日志框架，比如使用 Log4J， 即在类路径下提供 Log4J 配置文件，这样启动 Spring 容器才不会报错。

#### **ApplicationContext** 面向开发应用

![image-20191219180349451](/Users/dylan/Library/Application Support/typora-user-images/image-20191219180349451.png)

ApplicationContext 由 BeanFactory 派生而来，提供了更多面向实际应用的功能。 ApplicationContext 继承了 HierarchicalBeanFactory 和 ListableBeanFactory 接口，在此基础 上，还通过多个其他的接口扩展了 BeanFactory 的功能:

1. ClassPathXmlApplicationContext:默认从类路径加载配置文件
2. FileSystemXmlApplicationContext:默认从文件系统中装载配置文件
3. ApplicationEventPublisher:让容器拥有发布应用上下文事件的功能，包括容器启动事件、关闭事件等。
4. MessageSource:为应用提供 i18n 国际化消息访问的功能;
5. ResourcePatternResolver : 所 有 ApplicationContext 实 现 类 都 实 现 了 类 似 于PathMatchingResourcePatternResolver的功能，可以通过带前缀的 Ant 风格的资源文件路径装载 Spring 的配置文件。
6. LifeCycle:该接口是 Spring 2.0 加入的，该接口提供了 start()和 stop()两个方法，主要用于控制异步处理过程。在具体使用时，该接口同时被 ApplicationContext 实现及具体 Bean 实现， ApplicationContext 会将 start/stop 的信息传递给容器中所有实现了该接 口的 Bean，以达到管理和控制 JMX、任务调度等目的
7. ConfigurableApplicationContext 扩展于 ApplicationContext，它新增加了两个主要 的方法: refresh()和 close()，让 ApplicationContext 具有启动、刷新和关闭应用上下 文的能力。在应用上下文关闭的情况下调用 refresh()即可启动应用上下文，在已经启动 的状态下，调用 refresh()则清除缓存并重新装载配置信息，而调用 close()则可关闭应用 上下文。

#### **WebApplication** 体系架构

整个 Web 应用上下文对象将作为属性放置到 ServletContext 中，以便 Web 应用环境可以访问 Spring 应用上下文。

### Spring Bean作用域

singleton(单例)、prototype(原型)、request、session 和 global session

**singleton**:单例模式(多线程下不安全)

Spring IoC 容器中只会存在一个共享的 Bean 实例，无论有多少个 Bean 引用它，始终指向同一对象。该模式在多线程下是不安全的。Singleton 作用域是 Spring 中的缺省作用域，也可以显示的将 Bean 定义为 singleton 模式，配置为:

`<bean id="userDao" class="com.ioc.UserDaoImpl" scope="singleton"/>`

**prototype:**原型模式每次使用时创建

每次通过 Spring 容器获取 prototype 定义的 bean 时，容器都将创建一个新的 Bean 实例，每个 Bean 实例都有自己的属性和状态，而 singleton 全局只有一个对象。**根据经验，对有状态的 bean 使用 prototype 作用域，而对无状态的 bean 使用 singleton 作用域。**

**Request**:一次 **request** 一个实例

在一次 Http 请求中，容器会返回该 Bean 的同一实例。而对不同的 Http 请求则会 产生新的 Bean，而且该 bean 仅在当前 Http Request 内有效,当前 Http 请求结束，该 bean 实例也将会被销毁。

`<bean id="loginAction" class="com.cnblogs.Login" scope="request"/>`

 **session**

在一次 Http Session 中，容器会返回该 Bean 的同一实例。而对不同的 Session 请 求则会创建新的实例，该 bean 实例仅在当前 Session 内有效。同 Http 请求相同，每一次 session 请求创建新的实例，而不同的实例之间不共享属性，且实例仅在自己的 session 请求 内有效，请求结束，则实例将被销毁。

`<bean id="userPreference" class="com.ioc.UserPreference" scope="session"/>`

**global Session**

在一个全局的 Http Session 中，容器会返回该 Bean 的同一个实例，仅在使用 portlet context 时有效。

### Spring 生命周期

![image-20191219181500147](/Users/dylan/Library/Application Support/typora-user-images/image-20191219181500147.png)

1. 实例化一个 Bean，也就是我们常说的 new。

2. **IOC** 依赖注入按照 Spring 上下文对实例化的 Bean 进行配置，也就是 IOC 注入。

3. **setBeanName**实现,如果这个 Bean 已经实现了 BeanNameAware 接口，会调用它实现的 setBeanName(String)方法，此处传递的就是 Spring 配置文件中 Bean 的 id 值

4. **BeanFactoryAware** 实现, 如果这个 Bean 已经实现了 BeanFactoryAware 接口，会调用它实现的 setBeanFactory， setBeanFactory(BeanFactory)传递的是 Spring 工厂自身(可以用这个方式来获取其它 Bean， 只需在 Spring 配置文件中配置一个普通的 Bean 就可以)。

5. **ApplicationContextAware** 实现,如果这个 Bean 已经实现了 ApplicationContextAware 接口，会调用 setApplicationContext(ApplicationContext)方法，传入 Spring 上下文(同样这个方式也 可以实现步骤 4 的内容，但比 4 更好，因为 ApplicationContext 是 BeanFactory 的子接 口，有更多的实现方法)

6. **postProcessBeforeInitialization** 接口实现**-**初始化预处理,如果这个 Bean 关联了 BeanPostProcessor 接口，将会调用 postProcessBeforeInitialization(Object obj, String s)方法，BeanPostProcessor 经常被用 作是 Bean 内容的更改，并且由于这个是在 Bean 初始化结束时调用那个的方法，也可以被应 用于内存或缓存技术。
7. init-method, 如果 Bean 在 Spring 配置文件中配置了 init-method 属性会自动调用其配置的初始化方法。

8. postProcessAfterInitialization,如果这个 Bean 关联了 BeanPostProcessor 接口，将会调用 postProcessAfterInitialization(Object obj, String s)方法。 

   注:以上工作完成以后就可以应用这个 Bean 了，那这个 Bean 是一个 Singleton 的，所以一 般情况下我们调用同一个 id 的 Bean 会是在内容地址相同的实例，当然在 Spring 配置文件中 也可以配置非 Singleton。

9. **Destroy** 过期自动清理阶段 当 Bean 不再需要时，会经过清理阶段，如果 Bean 实现了 DisposableBean 这个接口，会调用那个其实现的 destroy()方法;

10. **destroy-method** 自配置清理  最后，如果这个 Bean 的 Spring 配置中配置了 destroy-method 属性，会自动调用其配置的销毁方法。

11. bean 标签有两个重要的属性(init-method 和 destroy-method)。用它们你可以自己定制 初始化和注销方法。它们也有相应的注解(@PostConstruct 和@PreDestroy)。

    `<bean id="" class="" init-method="初始化方法" destroy-method="销毁方法">`

### Spring 依赖注入四种方式

1.构造器注入

```java
/*带参数，方便利用构造器进行注入*/
public CatDaoImpl(String message){
	this. message = message;
}
<bean id="CatDaoImpl" class="com.CatDaoImpl"> 
  <constructor-arg value="message"></constructor-arg>
</bean>
```

2.setter方法注入

```java
public class Id {
	private int id;
	public int getId() { return id; }
	public void setId(int id) { this.id = id; }
}
```

```xml
<bean id="id" class="com.id "> 
  <property name="id" value="123"></property>
</bean>
```

3.静态工厂注入???

```java
/*
 *静态工厂注入
 *通过调用静态工厂的方法来获取自己需要的对象
 *为了让spring管理所有对象，我们不能直接通过"工程类.静态方法()"来获取对象
 *而是依然通过 spring 注入的形式获取:
*/
public class DaoFactory { //静态工厂
	public static final FactoryDao getStaticFactoryDaoImpl(){
		return new StaticFacotryDaoImpl(); }
	}
	public class SpringAction {
    //注入对象 
		private FactoryDao staticFactoryDao; 
    //注入对象的 set 方法
		public void setStaticFactoryDao(FactoryDao staticFactoryDao) { 
      this.staticFactoryDao = staticFactoryDao;
		} 
}

```

```xml
<!-- factory-method="getStaticFactoryDaoImpl"指定调用哪个工厂方法 -->

<bean name="springAction" class="SpringAction" >
		<!--使用静态工厂的方法注入对象,对应下面的配置文件-->
		<property name="staticFactoryDao" ref="staticFactoryDao"></property>
</bean>
<!--此处获取对象的方式是从工厂类中获取静态方法--> 
<bean name="staticFactoryDao" class="DaoFactory" factory-method="getStaticFactoryDaoImpl"></bean>
```

4.实例工厂:获取对象实例的方法不是静态的，所以你需要首先 new 工厂类，再调用普通的 实例方法

```java
public class DaoFactory { //实例工厂 
  public FactoryDao getFactoryDaoImpl(){
		return new FactoryDaoImpl();
  }
}
public class SpringAction {
	private FactoryDao factoryDao; //注入对象
	public void setFactoryDao(FactoryDao factoryDao) { 
    this.factoryDao = factoryDao;
	}
}
```

```xml
<bean name="springAction" class="SpringAction"> 
  <!--使用实例工厂的方法注入对象,对应下面的配置文件--> 
  <property name="factoryDao" ref="factoryDao"></property>
</bean> 
<!--此处获取对象的方式是从工厂类中获取实例方法-->
<bean name="daoFactory" class="com.DaoFactory"></bean> 
<bean name="factoryDao" factory-bean="daoFactory"factory-method="getFactoryDaoImpl"></bean>
```



## Spring APO 原理

### 概念

"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块， 并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共 同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。

使用"横切"技术，AOP 把软件系统分为两个部分:核心关注点和横切关注点。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生 在核心关注点的多处，而各处基本相似，比如权限认证、日志、事物。AOP 的作用在于分离系统 中的各种关注点，将核心关注点和横切关注点分离开来。

AOP 主要应用场景有:

1. Authentication 权限
2. Caching 缓存
3. Context passing 内容传递
4. Error handling 错误处理
5. Lazy loading 懒加载
6. Debugging 调试
7. logging, tracing, profiling and monitoring
8. Performance optimization 性能优化
9. Persistence 持久化
10. Resource pooling 资源池
11. Synchronization 同步
12. Transactions 事务

### 核心概念

1、切面(aspect):类是对物体特征的抽象，切面就是对横切关注点的抽象

2、横切关注点:对哪些方法进行拦截，拦截后怎么处理，这些关注点称之为横切关注点。

3、连接点(joinpoint):被拦截到的点，因为 Spring 只支持方法类型的连接点，所以在 Spring中连接点指的就是被拦截到的方法，实际上连接点还可以是字段或者构造器。 

4、切入点(pointcut):对连接点进行拦截的定义

5、通知(advice):所谓通知指的就是指拦截到连接点之后要执行的代码，通知分为前置、后置、 异常、最终、环绕通知五类。

6、目标对象:代理的目标对象 

7、织入(weave):将切面应用到目标对象并导致代理对象创建的过程

8、引入(introduction):在不修改代码的前提下，引入可以在运行期为类动态地添加一些方法 或字段。

### 两种代理方式

**1. JDK动态接口代理** 

 JDK 动态代理主要涉及到 java.lang.reflect 包中的两个类:Proxy 和 InvocationHandler。 InvocationHandler 是一个接口，通过实现该接口定义横切逻辑，并通过反射机制调用目标类 的代码，动态将横切逻辑和业务逻辑编制在一起。Proxy 利用 InvocationHandler 动态创建 一个符合某一接口的实例，生成目标类的代理对象。

**2. **CGLib 动态代理

CGLib 全称为 Code Generation Library，是一个强大的高性能，高质量的代码生成类库， 可以在运行期扩展 Java 类与实现 Java 接口，CGLib 封装了 asm，可以再运行期动态生成新 的 class。和 JDK 动态代理相比较:JDK 创建代理有一个限制，就是只能为接口创建代理实例， 而对于没有通过接口定义业务方法的类，则可以通过 CGLib 创建动态代理。

```java
@Aspect
public class TransactionDemo {
	@Pointcut(value="execution(* com.yangxin.core.service.*.*.*(..))")
	public void point(){ }
@Before(value="point()")
public void before(){ System.out.println("transaction begin");
}
public void after(){ System.out.println("transaction commit");
}
@Around("point()")
public void around(ProceedingJoinPoint joinPoint) throws Throwable{ System.out.println("transaction begin");
joinPoint.proceed();
System.out.println("transaction commit");
} }
```

## Spring MVC原理

Spring 的模型-视图-控制器(MVC)框架是围绕一个 DispatcherServlet 来设计的，这个 Servlet 会把请求分发给各个处理器，并支持可配置的处理器映射、视图渲染、本地化、时区与主题渲染 等，甚至还能支持文件上传。

# 9.计算机网络

## 网络七层架构

1. 物理层:主要定义物理设备标准，如网线的接口类型、光纤的接口类型、各种传输介质的传输速率 等。它的主要作用是传输比特流(就是由 1、0 转化为电流强弱来进行传输,到达目的地后在转化为 1、0，也就是我们常说的模数转换与数模转换)。这一层的数据叫做比特。
2. 数据链路层:主要将从物理层接收的数据进行 **MAC 地址(网卡的地址)的封装与解封装**。常把这 一层的数据叫做**帧**。在这一层工作的设备是**交换机**，数据通过交换机来传输。
3. 网络层:主要将从下层接收到的数据进行 **IP 地址的封装与解封装**。在这一层工 作的设备是**路由器**，常把这一层的数据叫做**数据包**。
4. 传输层:定义了一些传输数据的协议和端口号(WWW 端口 80 等)，如:TCP(传输控制协议， 传输效率低，可靠性强，用于传输可靠性要求高，数据量大的数据)，UDP(用户数据报协议， 与 TCP 特性恰恰相反，用于传输可靠性要求不高，数据量小的数据，如 QQ 聊天数据就是通过这种方式传输的)。 主要是将从下层接收的数据进行分段进行传输，到达目的地址后在进行重组。 常常把这一层数据叫做**段**。
5. 会话层:通过传输层(端口号:传输端口与接收端口)建立数据传输的通路。主要在你的系统之间 发起会话或或者接受会话请求(设备之间需要互相认识可以是 IP 也可以是 MAC 或者是主机名)
6. 表示层:主要是进行对接收的数据进行解释、加密与解密、压缩与解压缩等(也就是把计算机能够 识别的东西转换成人能够能识别的东西(如图片、声音等))
7. 应用层 主要是一些终端的应用，比如说 **FTP(各种文件下载)，WEB(IE 浏览)**，QQ 之类的(你 就把它理解成我们在电脑屏幕上可以看到的东西.就 是终端应用)。

## TCP/IP原理

TCP/IP 协议不是 TCP 和 IP 这两个协议的合称，而是指因特网整个 TCP/IP 协议族。从协议分层 模型方面来讲，TCP/IP 由四个层次组成:网络接口层、网络层、传输层、应用层。

![image-20191220101746073](/Users/dylan/Library/Application Support/typora-user-images/image-20191220101746073.png)



1. 网络访问层(Network Access Layer)在 TCP/IP 参考模型中并没有详细描述，只是指出主机 必须使用某种协议与网络相连。

2. 网络层(Internet Layer)是整个体系结构的关键部分，其功能是使主机可以把分组发往任何网 络，并使分组独立地传向目标。这些分组可能经由不同的网络，到达的顺序和发送的顺序也 可能不同。高层如果需要顺序收发，那么就必须自行处理对分组的排序。互联网层使用因特 网协议(IP，Internet Protocol)。
3. 传输层(Tramsport Layer)使源端和目的端机器上的对等实体可以进行会话。在这一层定义了 两个端到端的协议:传输控制协议(TCP，Transmission Control Protocol)和用户数据报协 议(UDP，User Datagram Protocol)。TCP 是面向连接的协议，它提供可靠的报文传输和对 上层应用的连接服务。为此，除了基本的数据传输外，它还有可靠性保证、流量控制、多路复用、优先权和安全性控制等功能。UDP 是面向无连接的不可靠传输的协议，主要用于不需要 TCP 的排序和流量控制等功能的应用程序。 		

4. 应用层(Application Layer)包含所有的高层协议，包括:

   虚拟终端协议(TELNET， TELecommunications NETwork)

   文件传输协议(FTP，File Transfer Protocol)

   电子邮件传输协议(SMTP，Simple Mail Transfer Protocol)

   域名服务(DNS，Domain NameService)

   网上新闻传输协议(NNTP，Net News Transfer Protocol)

   超文本传送协议 (HTTP，HyperText Transfer Protocol)等。

## TCP**三次握手**/四次挥手
 TCP 在传输之前会进行三次沟通，一般称为“三次握手”，传完数据断开的时候要进行四次沟通，一般

称为“四次挥手”。

### 数据包说明

1. 源端口号( 16 位):它(连同源主机 IP 地址)标识源主机的一个应用进程。

2. 目的端口号( 16 位):它(连同目的主机 IP 地址)标识目的主机的一个应用进程。这两个值

   加上 IP 报头中的源主机 IP 地址和目的主机 IP 地址唯一确定一个 TCP 连接。

3. 顺序号 seq( 32 位):用来标识从 TCP 源端向 TCP 目的端发送的数据字节流，它表示在这个报文段中的第一个数据字节的顺序号。如果将字节流看作在两个应用程序间的单向流动，则 TCP 用顺序号对每个字节进行计数。序号是 32bit 的无符号数，序号到达 2 的 32 次方 - 1 后 又从 0 开始。当建立一个新的连接时， SYN 标志变 1 ，顺序号字段包含由这个主机选择的该 连接的初始顺序号 ISN ( Initial Sequence Number )。

4. 确认号 ack( 32 位):包含发送确认的一端所期望收到的下一个顺序号。因此，确认序号应当 是上次已成功收到数据字节顺序号加 1 。只有 ACK 标志为 1 时确认序号字段才有效。 TCP 为 应用层提供全双工服务，这意味数据能在两个方向上独立地进行传输。因此，连接的每一端必 须保持每个方向上的传输数据顺序号。

5. TCP 报头长度( 4 位):给出报头中 32bit 字的数目，它实际上指明数据从哪里开始。需要这 个值是因为任选字段的长度是可变的。这个字段占 4bit ，因此 TCP 最多有 60 字节的首部。然 而，没有任选字段，正常的长度是 20 字节。

6. 保留位( 6 位):保留给将来使用，目前必须置为 0 。

7. 控制位( control flags ， 6 位):在 TCP 报头中有 6 个标志比特，它们中的多个可同时被设置为 1 。依次为:

   -  URG :为 1 表示紧急指针有效，为 0 则忽略紧急指针值。
   -  ACK :为 1 表示确认号有效，为 0 表示报文中不包含确认信息，忽略确认号字段。
   - PSH :为 1 表示是带有 PUSH 标志的数据，指示接收方应该尽快将这个报文段交给应用层而不用等待缓冲区装满。
   - RST :用于复位由于主机崩溃或其他原因而出现错误的连接。它还可以用于拒绝非法的报文段和拒绝连接请求。一般情况下，如果收到一个 RST 为 1 的报文，那么一定发生了某些问题。
   - SYN :同步序号，为 1 表示连接请求，用于建立连接和使顺序号同步( synchronize )。
   - FIN :用于释放连接，为 1 表示发送方已经没有数据发送了，即关闭本方数据流。

8. 窗口大小( 16 位):数据字节数，表示从确认号开始，本报文的源方可以接收的字节数，即源 方接收窗口大小。窗口大小是一个 16bit 字段，因而窗口大小最大为 65535 字节。

9. 校验和( 16 位):此校验和是对整个的 TCP 报文段，包括 TCP 头部和 TCP 数据，以 16 位字 进行计算所得。这是一个强制性的字段，一定是由发送端计算和存储，并由接收端进行验证。

10. 紧急指针(16位):只有当URG标志置1时紧急指针才有效。TCP的紧急方式是发送端向另 一端发送紧急数据的一种方式。

11. 选项:最常见的可选字段是最长报文大小，又称为MSS(MaximumSegmentSize)。每个连 接方通常都在通信的第一个报文段(为建立连接而设置 SYN 标志的那个段)中指明这个选项， 它指明本端所能接收的最大长度的报文段。选项长度不一定是 32 位字的整数倍，所以要加填充 位，使得报头长度成为整字数。

12. 数据: TCP 报文段中的数据部分是可选的。在一个连接建立和一个连接终止时，双方交换的报 文段仅有 TCP 首部。如果一方没有数据要发送，也使用没有任何数据的首部来确认收到的数 据。在处理超时的许多情况中，也会发送不带任何数据的报文段。

    ![image-20191220103223529](/Users/dylan/Library/Application Support/typora-user-images/image-20191220103223529.png)



### 三次握手

1. 第一次握手:主机 A 发送位码为 syn=1,随机产生 seq number=1234567 的数据包到服务器，主机 B由 SYN=1 知道，A 要求建立联机;
2. 第二次握手:主机 B 收到请求后要确认联机信息，向 A 发送 ack number=(主机 A 的seq+1),syn=1,ack=1,随机产生 seq=7654321 的包

3. 第三次握手:主机 A 收到后检查 ack number 是否正确，即第一次发送的 seq number+1,以及位码ack 是否为 1，若正确，主机 A 会再发送 ack number=(主机 B 的 seq+1),ack=1，主机 B 收到后确认

### 四次挥手

TCP 建立连接要进行三次握手，而断开连接要进行四次。这是由于 TCP 的半关闭造成的。因为 TCP 连接是全双工的(即数据可在两个方向上同时传递)所以进行关闭时每个方向上都要单独进行关闭。这个单方向的关闭就叫半闭.当一方完成它的数据发送任务，就发送一个 FIN 来向另一方通告将要终止这个 方向的连接。

1) 关闭客户端到服务器的连接:首先客户端 A 发送一个 FIN，用来关闭客户到服务器的数据传送， 然后等待服务器的确认。其中终止标志位 FIN=1，序列号 seq=u
2) 服务器收到这个 FIN，它发回一个 ACK，确认号 ack 为收到的序号加 1。
3) 关闭服务器到客户端的连接:也是发送一个 FIN 给客户端。
4) 客户段收到 FIN 后，并发回一个 ACK 报文确认，并将确认序号 seq 设置为收到序号加 1。

首先进行关闭的一方将执行主动关闭，而另一方执行被动关闭。

![image-20191220103857002](/Users/dylan/Library/Application Support/typora-user-images/image-20191220103857002.png)

主机 A 发送 FIN 后，进入终止等待状态， 服务器 B 收到主机 A 连接释放报文段后，就立即 给主机 A 发送确认，然后服务器 B 就进入 close-wait 状态，此时 TCP 服务器进程就通知高层应用进程，因而从 A 到 B 的连接就释放了。此时是“半关闭”状态。即 A 不可以发送给 B，但是 B 可以发送给 A。此时，若 B 没有数据报要发送给 A 了，其应用进程就通知 TCP 释放连接，然后发送给 A 连接释放报文段，并等待确认。A 发送确认后，进入 time-wait，注 意，此时 TCP 连接还没有释放掉，然后经过时间等待计时器设置的 2MSL 后，A 才进入到 close 状态。



## http原理

### 传输流程

1. 地址解析

   如用客户端浏览器请求这个页面:`http ://localhost.com:8080/index.html` 

   从中分解出协议名、主机名、端口、对象路径等部分，对于我们的这个地址，解析得到的结果如下: 

   ```
   协议名:http
   主机名:localhost.com
   端口:8080
   对象路径:/index.htm
   ```

   在这一步，需要域名系统 DNS 解析域名 localhost.com,得主机的 IP 地址。

2. 封装 **HTTP** 请求数据包 把以上部分结合本机自己的信息，封装成一个 HTTP 请求数据包

3. 封装成 **TCP** 包并建立连接,封装成 TCP 包，建立 TCP 连接(TCP 的三次握手)

4. **客户机发送请求命令**:建立连接后，客户机发送一个请求给服务器，请求方式的格式为:统一资源标识符(URL)、协议版本号，后边是 MIME 信息包括请求修饰符、客户机信息和可内容。

5. **服务器响应** 服务器接到请求后，给予相应的响应信息，其格式为一个状态行，包括信息的协议版本号、一个成功或错误的代码，后边是 MIME 信息包括服务器信息、实体信息和可能的内容。 

6. 服务器关闭 **TCP** 连接

   一般情况下，一旦 Web 服务器向浏览器发送了请求数据，它就要关闭 TCP 连 接，然后如果浏览器或者服务器在其头信息加入了这行代码 Connection:keep-alive，TCP 连接在发送 后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求。保持连接节省了为每个请求 建立新连接所需的时间，还节约了网络带宽。

### http状态

![image-20191220125047845](/Users/dylan/Library/Application Support/typora-user-images/image-20191220125047845.png)

![image-20191220125510342](/Users/dylan/Library/Application Support/typora-user-images/image-20191220125510342.png)

![image-20191220125524752](/Users/dylan/Library/Application Support/typora-user-images/image-20191220125524752.png)

![image-20191220125545931](/Users/dylan/Library/Application Support/typora-user-images/image-20191220125545931.png)

### **HTTPS**

[小灰的讲解](https://blog.csdn.net/bjweimengshu/article/details/87706654)

HTTPS(全称:Hypertext Transfer Protocol over Secure Socket Layer)，是以安全为目标的 HTTP 通道，简单讲是 HTTP 的安全版。即 HTTP 下加入 SSL 层，HTTPS 的安全基础是 SSL。其所用的端口号是 443。 过程大致如下:

1. 建立连接获取证书

   SSL 客户端通过 TCP 和服务器建立连接之后(443 端口)，并且在一般的 tcp 连接协商(握 手)过程中请求证书。即客户端发出一个消息给服务器，这个消息里面包含了自己可实现的算 法列表和其它一些需要的消息，SSL 的服务器端会回应一个数据包，这里面确定了这次通信所 需要的算法，然后服务器向客户端返回证书。(证书里面包含了服务器信息:域名、申请证书 的公司，公共秘钥)。

2. 证书验证

   Client 在收到服务器返回的证书后，判断签发这个证书的公共签发机构，并使用这个机构的公 共秘钥确认签名是否有效，客户端还会确保证书中列出的域名就是它正在连接的域名。

3. 数据加密和传输 如果确认证书有效，那么生成对称秘钥并使用服务器的公共秘钥进行加密。然后发送给服务 器，服务器使用它的私钥对它进行解密，这样两台计算机可以开始进行对称加密进行通信。 	

![image-20191220132256997](/Users/dylan/Library/Application Support/typora-user-images/image-20191220132256997.png)

### CDN原理 Content Delivery Network

CND 一般包含分发服务系统、负载均衡系统和管理系统

1. 分发服务系统

   其基本的工作单元就是各个 Cache 服务器。

   - 负责直接响应用户请求，将内容快速分发到用户;
   - 负责内容更新，保证和源站内容的同步。
   - 向上层的管理调度系统反馈各个 Cache 设备的健康状况、响应情况、内容缓存状况等，以便管理调度系统能够根据设定的策略决定由 哪个 Cache 设备来响应用户的请求。

   根据内容类型和服务种类的不同，分发服务系统分为多个子服务系统，如:**网页加速服务**、**流媒体加速服务、应用加速服务等**。每个子服务系统都是一个分布式的服务集群，由功能类似、地域接近的分布部署的 Cache 集群组成。

2. 负载均衡系统:

   负载均衡系统是整个 CDN 系统的中枢。

   负责对所有的用户请求进行调度，确定提供给用户的最终访问 地址。 

   使用分级实现。最基本的两极调度体系包括全局负载均衡(GSLB)和本地负载均衡(SLB)。

   - GSLB 根据用户地址和用户请求的内容，主要根据就近性原则，**确定向用户服务的节点**。一般通过 DNS 解析或者应用层重定向(Http 3XX 重定向)的方式实现。
   - SLB 主要负责节点内部的负载均衡。当用户请求从 GSLB 调度到 SLB 时，SLB 会根据节点内各个 Cache 设备的工作状况和内容分布情况等**对用户请求重定向**。SLB 的实现有四层调度(LVS)、七层调 度(Nginx)和链路负载调度等

3. 管理系统,分为运营管理和网络管理子系统。

   - 运营管理是对 CDN 系统的业务管理，负责处理业务层面的与外界系统交互所必须的一些收集、整理、 交付工作。包括用户管理、产品管理、计费管理、统计分析等。

   - 网络管理系统实现对 CDN 系统的设备管理、拓扑管理、链路监控和故障管理，为管理员提供对全网资源的可视化的集中管理，通常用 web 方式实现。

![image-20191220135136392](/Users/dylan/Library/Application Support/typora-user-images/image-20191220135136392.png)



# 日志

**10.1.1. Slf4j**

slf4j 的全称是 Simple Loging Facade For Java，即它仅仅是一个为 Java 程序提供日志输出的统一接 口，并不是一个具体的日志实现方案，就比如 JDBC 一样，只是一种规则而已。所以单独的 slf4j 是不 能工作的，必须搭配其他具体的日志实现方案，比如 apache 的 org.apache.log4j.Logger，jdk 自带 的 java.util.logging.Logger 等。

**10.1.2. Log4j**

Log4j 是 Apache 的一个开源项目，通过使用 Log4j，我们可以控制日志信息输送的目的地是控制台、 文件、GUI 组件，甚至是套接口服务器、NT 的事件记录器、UNIX Syslog 守护进程等;我们也可以控 制每一条日志的输出格式;通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。 

Log4j 由三个重要的组成构成:日志记录器(Loggers)，输出端(Appenders)和日志格式化器(Layout)。
1.Logger:控制要启用或禁用哪些日志记录语句，并对日志信息进行级别限制 
2.Appenders : 指定了日志将打印到控制台还是文件中
3.Layout : 控制日志信息的显示格式

Log4j 中将要输出的 Log 信息定义了 5 种级别，依次为 DEBUG、INFO、WARN、ERROR 和 FATAL， 当输出时，只有级别高过配置中规定的 级别的信息才能真正的输出，这样就很方便的来配置不同情况 下要输出的内容，而不需要更改代码。

**10.1.3. LogBack**

简单地说，Logback 是一个 Java 领域的日志框架。它被认为是 Log4J 的继承人。

Logback 主要由三个模块组成:logback-core，logback-classic。logback-access

- logback-core 是其它模块的基础设施，其它模块基于它构建，显然，logback-core 提供了一些关键的 通用机制。

- logback-classic 的地位和作用等同于 Log4J，它也被认为是 Log4J 的一个改进版，并且它实现了简单 日志门面 SLF4J;

- logback-access 主要作为一个与 Servlet 容器交互的模块，比如说 tomcat 或者 jetty，提供一些与 HTTP 访问相关的功能。

  **10.1.3.1.** **Logback** **优点**

- 同样的代码路径，Logback 执行更快

- 更充分的测试

- 原生实现了 SLF4J API(Log4J 还需要有一个中间转换层)

- 内容更丰富的文档

- 支持 XML 或者 Groovy 方式配置

- 配置文件自动热加载

- 从 IO 错误中优雅恢复

-  自动删除日志归档

- 自动压缩日志成为归档文件

- 支持 Prudent 模式，使多个 JVM 进程能记录同一个日志文件

- 支持配置文件中加入条件判断来适应不同的环境

- 更强大的过滤器

- 支持 SiftingAppender(可筛选 Appender)

- 异常栈信息带有包信息

**10.1.4. ELK**

ELK 是软件集合 Elasticsearch、Logstash、Kibana 的简称，由这三个软件及其相关的组件可以打 造大规模日志实时处理系统。

- Elasticsearch 是一个基于 Lucene 的、支持全文索引的分布式存储和索引引擎，主要负责将 日志索引并存储起来，方便业务方检索查询。
- Logstash 是一个日志收集、过滤、转发的中间件，主要负责将各条业务线的各类日志统一收 集、过滤后，转发给 Elasticsearch 进行下一步处理。
- Kibana 是一个可视化工具，主要负责查询 Elasticsearch 的数据并以可视化的方式展现给业 务方，比如各类饼图、直方图、区域图等。



# zookeeper

**Zookeeper** 概念

Zookeeper 是一个分布式协调服务，可用于服务发现，分布式锁，分布式领导选举，配置管理等。 Zookeeper 提供了一个类似于 Linux 文件系统的树形结构(可认为是轻量级的内存文件系统，但 只适合存少量信息，完全不适合存储大量文件或者大文件)，同时提供了对于每个节点的监控与 通知机制。

**Zookeeper** 角色
 Zookeeper 集群是一个基于主从复制的高可用集群，每个服务器承担如下三种角色中的一种

- Leader

  一个 Zookeeper 集群同一时间只会有一个实际工作的 Leader，它会发起并维护与各 Follwer 及 Observer 间的心跳。所有的写操作必须要通过 Leader 完成再由 Leader 将写操作广播给其它服务器。只要有超过 半数节点(不包括 observer 节点)写入成功，该写请求就会被提交(类 2PC 协议)。

- Follower
  1. 一个 Zookeeper 集群可能同时存在多个 Follower，它会响应 Leader 的心跳，
  2. Follower 可直接处理并返回客户端的读请求，同时会将写请求转发给 Leader 处理，
  3. 并且负责在 Leader 处理写请求时对请求进行投票。

**Observer**

角色与 Follower 类似，但是无投票权。Zookeeper 需保证高可用和强一致性，为了支持更多的客 户端，需要增加更多 Server;Server 增多，投票阶段延迟增大，影响性能;引入 Observer， Observer 不参与投票; Observers 接受客户端的连接，并将写请求转发给 leader 节点; 加入更 多 Observer 节点，提高伸缩性，同时不影响吞吐率。

![image-20191221103444926](/Users/dylan/Library/Application Support/typora-user-images/image-20191221103444926.png)

### ZAB协议

Zookeeper atomic broadcast

事务编号 **Zxid**(事务请求计数器**+ epoch**)

在 ZAB ( ZooKeeper Atomic Broadcast , ZooKeeper 原子消息广播协议) 协议的事务编号 Zxid 设计中，Zxid 是一个 64 位的数字，其中低 32 位是一个简单的单调递增的计数器，针对客户端每一个事务请求，计数器加 1;而高 32 位则代表 Leader 周期 epoch 的编号，每个当选产生一个新 的 Leader 服务器，就会从这个 Leader 服务器上取出其本地日志中最大事务的 ZXID，并从中读取 epoch 值，然后加 1，以此作为新的 epoch，并将低 32 位从 0 开始计数。

Zxid(Transaction id)类似于 RDBMS 中的事务 ID，用于标识一次更新操作的 Proposal(提议) ID。为了保证顺序性，该 zkid 必须单调递增。

**epoch**

可以理解为当前集群所处的年代或者周期，每个 leader 就像皇帝，都有自己的年号，所 以每次改朝换代，leader 变更之后，都会在前一个年代的基础上加 1。这样就算旧的 leader 崩溃 恢复之后，也没有人听他的了，因为 follower 只听从当前年代的 leader 的命令。

**Zab** 协议有两种模式**-**恢复模式(选主)、广播模式(同步)

当服务启动或者在领导者崩溃后，Zab 就进入了恢复模式，当领导者被选举出来，且大多数 Server 完成了和 leader 的状 态同步以后，恢复模式就结束了。状态同步保证了 leader 和 Server 具有相同的系统状态。

**ZAB**协议**4**阶段

1. **Leader election**(选举阶段**-**选出准 **Leader**)

   节点在一开始都处于选举阶段，只要有一个节点得到超半数节点的票数，它就可以当选准 leader。只有到达 广播阶段(broadcast) 准 leader 才会成 为真正的 leader。这一阶段的目的是就是为了选出一个准 leader

2. **Discovery**(发现阶段**-**接受提议、生成 **epoch**、接受 **epoch**)

   在这个阶段，followers 跟准 leader 进行通信，同步 followers 最近接收的事务提议。这个一阶段的主要目的是发现当前大多数节点接收的最新提议，并且 准 leader 生成新的 epoch，让 followers 接受，更新它们的 accepted Epoch

   一个 follower 只会连接一个 leader，如果有一个节点 f 认为另一个 follower p 是 leader，f 在尝试连接 p 时会被拒绝，f 被拒绝之后，就会进入重新选举阶段。

3. **Synchronization**(同步阶段**-**同步 **follower** 副本)

   同步阶段主要是利用 leader 前一阶段获得的最新提议历史， 同步集群中所有的副本。只有当大多数节点都同步完成，准 leader 才会成为真正的 leader。 follower 只会接收 zxid 比自己的 lastZxid 大的提议。

4. **Broadcast**(广播阶段**-leader** 消息广播) 

   到了这个阶段，Zookeeper 集群才能正式对外提供事务服务，


 ZAB 提交事务并不像 2PC 一样需要全部 follower 都 ACK，只需要得到超过半数的节点的 ACK 就可以了。

并且 leader 可以进行消息广播。同时如果有新的节点加入，还需要对新节点进行同步。 

**ZAB** 协议 **JAVA** 实现(**FLE-**发现阶段和同步合并为 **Recovery Phase**(恢复阶段))

协议的 Java 版本实现跟上面的定义有些不同，选举阶段使用的是 Fast Leader Election(FLE)， 它包含了 选举的发现职责。因为 FLE 会选举拥有最新提议历史的节点作为 leader，这样就省去了 发现最新提议的步骤。实际的实现将 发现阶段 和 同步合并为 Recovery Phase(恢复阶段)。所 以，ZAB 的实现只有三个阶段:Fast Leader Election;Recovery Phase;Broadcast Phase。

**11.1.1.2.** **投票机制**

每个 server 首先给自己投票，然后用自己的选票和其他 server 选票对比，权重大的胜出，使用权重较大的更新自身选票箱。具体选举过程如下:

每个 Server 启动以后都询问其它的 Server 它要投票给谁。对于其他 server 的询问， server 每次根据自己的状态都回复自己推荐的 leader 的 id 和上一次处理事务的 zxid(系统启动时每个 server 都会推荐自己) 收到所有 Server 回复以后，就计算出 zxid 最大的哪个 Server，并将这个 Server 相关信 息设置成下一次要投票的 Server。 

计算这过程中获得票数最多的的 sever 为获胜者，如果获胜者的票数超过半数，则改 server 被选为 leader。否则，继续这个过程，直到 leader 被选举出来

leader 就会开始等待 server 连接 
Follower 连接 leader，将最大的 zxid 发送给 leader 
Leader 根据 follower 的 zxid 确定同步点，至此选举阶段完成.
选举阶段完成 Leader 同步后通知 follower 已经成为 uptodate 状态 									 								 Follower 收到 uptodate 消息后，又可以重新接受 client 的请求进行服务了 									 							 						 					 								 							 						 					 				