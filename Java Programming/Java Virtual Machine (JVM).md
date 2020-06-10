# 0 JVM的内存结构
<div align=center>
<img src="https://raw.githubusercontent.com/KOVERcjm/Pictures/master/20191230154146.png"/>
<br />
JVM内存结构图例
</div>
![Java Memory Model](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/jvm03-20200610165252836.png)

# 1 Run-Time Data Areas- 运行时数据区域

若干程序运行期间所使用到的运行时数据区域，包括程序计数器、堆、虚拟机栈、方法区和本地方法栈。这些区域所使用的内存可不连续，可调节初始固定/最大最小容量。
Various run-time data areas that are used during the execution of a program, including PC register, heap, stack, method area and native method stack. The memory of areas does not need to be contiguous, and the initial fixed size or the maximum and minimum size may be chosen.

![一文看懂JVM内存布局及GC原理](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/dd614bf56417939aa0e0694fedf2caa9.png)

## 1.1 Program Counter (PC) Register- 程序计数器
Java虚拟机支持多线程，每条线程有自己的PC寄存器；任意时刻，一条线程只会执行一个方法（当前方法）的代码。
The Java Virtual Machine supports multithreading, and each JVM thread has its own PC register; at any point, each JVM thread is exectuing the code of a single method, namely the current method.

## 1.2 Java Heap- Java堆
Java堆是可供各个线程共享的运行时内存区域，也是供所有类实例和数组对象分配内存的区域。Java堆在虚拟机启动的时候就被创建，它存储了被自动内存管理系统（垃圾收集器）所管理的各种对象。
The Java Virtual Machine has a heap that is shared among all JVM threads. The heap is the run-time data area from which memory for all class instances and arrays is allocated. The JVM heap is created on VM start-up. Heap storage for objects is reclaimed by an automatic storage management system (a.k.a a garbage collector).
**可能发生的异常**：【OutOfMemoryError】实际所需的堆超过了自动内存管理系统能提供的最大容量。
Associated exceptional conditions: [OutOfMemoryError] a computation requires more heap than can be made available by the automatic storage management system.

## 1.3 JVM Stacks- Java虚拟机栈
每条Java虚拟机线程有自己私有的Java虚拟机栈，这个栈和线程同时创建，用于存储栈帧。栈帧可在**堆**中分配，每个栈帧关联着一个方法。
Each Java Virtual Machine has a private JVM stack, created at the same time as the thread, which stores frames. Frames may be heap allocated, and each frame is created when a method is invoked and destored when its method invocation completes.
**可能发生的异常**：【StackOverflowError】线程请求分配的栈容量超过了最大容量；【OutOfMemoryError】在尝试扩展容量或创建新线程时，申请不到足够的内存。
Associated exceptional conditions: [StackOverflowError] the computation in a thread requires a larger JVM stack than is permitted; [OutOfMemoryError] insufficient memory can be made available to effect a stack's dynamically expansion or a new thread's creation.

## 1.4 Java Method Area- Java方法区
Java方法区是可供各个线程共享的运行时内存区域。方法区存储了每个类的结构信息，如运行时常量池、字段和方法数据、构造函数和普通方法的字节码内容，还包括一些在类、实例、接口初始化时用到的特殊方法。方法区在虚拟机启动的时候创建，是**堆**的逻辑组成部分。
The Java Virtual Machine has a method area that is shared among all JVM threads. The method area stores per-class structures such as the run-time constant pool, field and method data, and the code for methods and constructors, including the special methods used in class and interface initialization and in instance initialization. The method area is created on VM start-up, and it is a logically part of the heap.
**可能发生的异常**：【OutOfMemoryError】方法区的内存不能满足内存分配请求。
Associated exceptional conditions: [OutOfMemoryError] memory in the method area cannot be made available to satisfy an allocation request.

## 1.5 Native Method Stack- 本地方法栈
本地方法栈是一个传统栈（C栈）用以支持本地方法，在线程内创建的时候按线程分配。
Native method stack is a conventional stack (C stack) supports native methods, which is typically allocated per thread when each thread is created.
**可能发生的异常**：【StackOverflowError】线程请求分配的栈容量超过了最大容量；【OutOfMemoryError】在尝试扩展容量或创建新线程时，申请不到足够的内存。
Associated exceptional conditions: [StackOverflowError] the computation in a thread requires a larger native method stack than is permitted; [OutOfMemoryError] insufficient memory can be made available to effect a stack's dynamically expansion or a new thread's creation.

# 2 Garbage Collection- 垃圾回收
在运行时数据区域中，程序计数器、虚拟机栈和本地方法栈3个区域随线程而生灭；栈中的栈帧随方法的进出而出入栈；这几个区域的内存分配与回收具备确定性。而**堆**和**方法区**则不一样，只有在程序运行期间才能动态地分配这部分内存。通过垃圾回收机制，Java使开发者无需关注空间的创建与释放，在后台建立一个守护进程，保障程序正常运行。
对垃圾回收以及内存分配的了解，可以为排查内存溢出、泄露问题时，或是垃圾回收成为了系统达到更高并发量的瓶颈时，对其实施必要的监控和调节的理论依据。

> 本章总结自《深入理解Java虚拟机：JVM高级特性与最佳实践》、《深入Java虚拟机》以及知乎回答。

## 2.1 垃圾回收算法简述
在Java虚拟机规范中，并未明确JVM使用哪种垃圾回收算法。但任何一种垃圾回收算法都要完成：1）识别“垃圾”对象；2）回收堆空间。

## 2.2 识别“垃圾”对象
对于程序无法触及的对象，被认为是“垃圾”。因为它们不再影响程序的未来执行，不会再被使用。

### 2.2.0 【对照】引用计数算法
通过给对象添加一个引用计数器，每当有一个地方引用它时，计数器就加1；当引用失效时，计数器就减1；因此计数器为0的对象就是不可能再被使用的。如下例：
``` Java
public class ReferenceCountGC
{
    public String string1 = null;
    public String string2 = null;
    
    public void init()
    {
        string1 = new String("abc");   // 对象分配，此时的字符串对象“abc”引用计数计1
        string2 = string1;      // 引用赋值，此时字符串对象“abc”的引用计数加1，为2
        myFunction(string1);      // 引用传参，此时字符串对象“abc”的引用计数加1，为3；函数返回时减1，为2
        string1 = null;           // 除去对象引用，此时字符串对象“abc”的引用计数减1，为1
    }
    
    public void myFunction(String string)   {}
}
```
引用计数算法实现简单，判定效率高，但很难解决对象之间相互循环引用的情况，如下例：
``` Java
public class Main
{
    public static void main(String[] args)
    {
        MyObject myObject1 = new MyObject();
        MyObject myObject2 = new MyObject();
        
        myObject1.object = myObject2;
        myObject2.object = myObject1;
        
        myObject1 = null;
        myObject2 = null;
    }
}
```
通过对myObject1和myObject2置null，myObject1和myObject2所指向的对象已经不能再被访问到；但由于互相引用，它们的引用计数器都不为零，不能被垃圾回收。

### 2.2.1 GC Roots- 根对象集合
Java虚拟机通常通过建立一个根对象的集合，检查从这些根对象开始的可触及性来实现可达性分析。而Java虚拟机的根对象集合根据实现不同而不同，可作为GC Roots的对象包括下面几种：

- 虚拟机栈帧中的本地变量表中引用的对象
- 方法区中类静态属性引用的对象
- 方法区中常量引用的对象
- 本地方法栈中Native方法引用的对象

### 2.2.2 Reachability Analysis- 可达性分析
可达性分析算法的基本思路是通过GC Roots对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链（Reference Chain）。当一个对象到GC Roots没有任何引用链相连的时候，即从GC Roots到这个对象不可达时，则证明这个对象时不可用的，它们将被判定为可回收对象。

![一文看懂JVM内存布局及GC原理](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/754f1ab05e6527107cfd8578d98a80a4.png)


### 2.2.3 引用
为定义这样一类对象：当内存空间还足够高时，则能保留在内存之中；如果内存空间在进行垃圾收集后还是非常紧张，则可以抛弃这些对象。因此，Java对引用的概念进行了扩充，引用强度依次递减：

- 强引用：在程序代码中普遍存在的引用；只要强引用存在，GC永远不会回收掉被引用的对象。
- 软引用：还有用但非必须的对象；系统会在将要发生内存溢出异常之前，把这些对象列入回收范围二次回收；如果二次回收依旧没有足够内存，才会抛出异常。可通过SoftReference类实现，通过联合使用ReferenceQueue来管理。
- 弱引用：非必须对象；系统会在下次垃圾收集时回收掉弱引用关联的对象，无论内存是否足够。可通过WeakReference类实现，通过联合使用ReferenceQueue来管理。
- 虚引用：幽灵引用/幻影引用；对对象生存时间无影响，无法通过虚引用获得对象实例；虚引用唯一目的是在这个对象被回收时收到系统通知。可通过PhantomReference实现。

### 2.2.4 不可达对象的两次标记
真正宣告一个不可达对象的“死亡”，至少经历两次标记过程：如果对象在进行可达性分析后发现没有与GC Roots相连接的引用链，它将被*第一次标记*并进行一次筛选；经过筛选后，对象可在**finalize()**中通过与引用链上任何一个对象建立关联来拯救自己，被移出“即将回收”的集合，否则将被*第二次标记*。任何一个对象的finalize()方法只会被系统自动调用一次。

### 2.2.5 【拓展】“垃圾”常量与“垃圾”类
在方法区或HotSpot虚拟机中的永生代中，可以回收无用的常量与类。无用常量的识别和“垃圾”对象的识别类似：检测常量池中常量是否被系统中对象所引用。无用类的识别需要同时满足下列三个条件：

- 该类的所有实例都已经被回收，不在Java堆中存在；
- 加载该类的ClassLoade已经被回收；
- 该类对应的java.lang.Class对象已经没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。

## 2.3 垃圾回收算法思想
### 2.3.1 Mark-Sweep- 标记-清除算法
又称为Tracing算法，分为两个阶段：标记判定所有需要回收的对象，在标记完成之后统一回收所有被标记的对象。后续的收集算法都基于这种思路，并对其不足加以改善。其主要不足有二：一是两个过程的效率不高，二是清除之后会产生大量不连续的内存碎片。内存碎片太多可能会导致过程运行中需要分配较大对象时，无法找到足够的连续内存而不得不提前触发另一次垃圾收集。

![标记-清除算法图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/gc2.png)

### 2.3.2 Copying- 复制算法
为解决效率问题，复制算法将可用内存按容量分为大小相等的两块，每次只使用其中的一块。当这一块用完了，将还存活的对象复制到另一块上，再把已经使用过的那块内存空间一次清理掉。这样内存分配的时候不用考虑内存碎片等复杂情况，只要移动对顶指针，按顺序分配内存即可。这种算法实现简单运行高效，代价是将内存缩小一半。使用场景必须是对象存活率非常低才行。

![复制算法图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/gc3.png)

### 2.3.3 Mark-Compacting- 标记-整理算法
标记整理算法中，标记过程与“标记-清理”算法相同，但后续步骤是让所有蔗对象向一端移动，直接清理掉端边界以外的内存。

![标记-整理算法图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/gc4.png)

### 2.3.4 Generational Collection- 分代收集算法
当前商业虚拟机都使用分代收集算法。这种算法根据对象存活周期，将内存划分为几块，根据各个年代的特点来财去最适当的收集算法。

## 2.4 Java的垃圾分代回收机制
Java堆空间一般被分为三个部分，用以存储三类数据：

- 新生代（Young Generation）：刚创建的对象。很多局部变量等在新创建后很快会变为不可达对象。因此这块区域的存活对象少。
- 老年代（Tenured Generation）：存活了一段时间的对象。这块区域的存活对象多。
- 持久代（Permanent Generation，Java 8以后改为元空间metaspace）：静态文件，如Java类、方法等。

结合各个部分的特点，在新生代中将采取复制算法，在老年代中采取标记-整理算法。在新生代中，内存被分为三个部分：Eden区、From Survivor区和To Survivor区，默认比例为8:1:1。

![一文看懂JVM内存布局及GC原理](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/e4ff361d409b6939e6da49a06b6dc677.png)
分代回收机制工作原理如下：

1. Eden区对外提供堆内存，新创建的对象将被存储至Eden区；
2. 如果新创建的对象过大，将会直接被存储至老年代；
3. 当Eden区满、或达到一定比例时，触发**Minor GC**，将Eden区与From Survivor区（如果有）内的存活对象复制到To Survivor区；
4. 交换From和To区的标记，保证始终有一个Survivor区为空；
5. 当对象在Survivor区中经过多次（默认15）回收依旧存活的时候，将会被移入老年代；
6. 当To Survivor区存放不下Eden和From Survivor区中的存活对象是，将会把存活对象直接存放到老年代；
7. 当老年代满的时候，触发**Full GC**，对整个堆进行垃圾回收；
8. 当JVM超过98%的时间用来GC，并且回收了不到2%的堆内存时，将触发**OutOfMemory**异常。

## 2.5 【拓展】HotSpot VM中的垃圾回收
准确来说，HotSpot虚拟机中，存在着两大类垃圾回收：

- Partial GC   -   收集部分堆
  - Young GC   -   只收集新生代
  - Old GC     -   只收集老年代（CMS的concurrent collection）
  - Mixed GC   -   收集新生代及部分老年代（仅G1有此模式）
- Full GC      -   收集全部堆

## 2.6 垃圾回收器
### 2.6.1 Serial收集器

Serial收集器是一个单线程收集器，在JVM需要进行垃圾回收的时候，需暂停所有的用户线程等待回收结束（Stop-The-World）。它的优点是简单高效，在单CPU环境中，由于没有线程交互的开销，可以获得最高的单线程收集效率，适用于Client模式的虚拟机。
Serial Young / Serial Old收集器运行示意图：

![Serial收集器图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/6c281cf0.png)

### 2.6.2 ParNew收集器
ParNew收集器是Serial收集器的多线程版本，除了使用多线程之外，回收策略等各方面与Serial收集器一样。它是运行在Server模式下的虚拟机首选的新生代收集器，目前只有它和Serial收集器能配合CMS收集器工作。
ParNew / Serial Old收集器运行示意图：

![ParNew收集器图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/75122b84.png)

### 2.6.3 Parallel收集器
Parallel Scavenge收集器是一个新生代收集器，也是使用复制算法的并行多线程收集器；特点是达到一个可控制的吞吐量。吞吐量 = 运行用户代码时间 / (运行用户代码时间 + 垃圾收集时间)。而高吞吐量比较适用于在后台运算而不需要太多交互的任务。虚拟机会根据当前系统的运行情况收集性能监控信息，动态调整这些参数以提供最适合的停顿时间或最大的吞吐量，这种调节方式被称为GC自适应调节策略（GC Ergonomics）。

### 2.6.4 Serial Old收集器
Serial Old是Serial收集器的老年代版本，同样是单线程收集器，使用标记-整理算法。主要给Client模式下的虚拟机使用；在Server模式下，可以与Parallel Scavenge收集器搭配使用，还可以作为CMS收集器的后备预案。

![Serial Old收集器图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/6c281cf0-20200610165613699.png)

### 2.6.5 Parallel Old收集器
Parallel Old是一个老年代并行收集器，以吞吐量优先。
Parallel Scavenge / Parallel Old收集器运行示意图：

![Parallel收集器图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/b1800d45.png)

### 2.6.6 CMS收集器
CMS（Concurrent Mark Sweep）收集器是以牺牲吞吐量为代价来获得最短回收停顿时间的垃圾回收器，适用于对停顿比较敏感，并且有相对较多存活时间较长的对象(老年代较大)的应用程序。CMS 虽然减少了回收的停顿时间，但是降低了堆空间的利用率。由于采用了 Mark-Sweep 算法，因此经过 CMS 收集的堆会产生空间碎片；为了解决堆空间浪费问题，CMS 回收器不再采用简单的指针指向一块可用堆空间来为下次对象分配使用。而是把一些未分配的空间汇总成一个列表，当 JVM 分配对象空间的时候，会搜索这个列表找到足够大的空间来存放住这个对象。其工作步骤如下：

- 初始标记(STW initial mark)：在这个阶段，需要虚拟机停顿正在执行的任务，官方的叫法 STW(Stop The Word)。这个过程从垃圾回收的"根对象"开始，只扫描到能够和"根对象"直接关联的对象，并作标记。所以这个过程虽然暂停了整个 JVM，但是很快就完成了。
- 并发标记(Concurrent marking)：这个阶段紧随初始标记阶段，在初始标记的基础上继续向下追溯标记。并发标记阶段，应用程序的线程和并发标记的线程并发执行，所以用户不会感受到停顿。
- 并发预清理(Concurrent precleaning)：并发预清理阶段仍然是并发的。在这个阶段，虚拟机查找在执行并发标记阶段新进入老年代的对象(可能会有一些对象从新生代晋升到老年代，或者有一些对象被分配到老年代)。通过重新扫描，减少下一个阶段"重新标记"的工作，因为下一个阶段会 Stop The World。
- 重新标记(STW remark)：这个阶段会暂停虚拟机，回收器线程扫描在 CMS 堆中剩余的对象。扫描从"跟对象"开始向下追溯，并处理对象关联。
- 并发清理(Concurrent sweeping)：清理垃圾对象，这个阶段回收器线程和应用程序线程并发执行。
- 并发重置(Concurrent reset)：这个阶段，重置 CMS 回收器的数据结构，等待下一次垃圾回收。
CMS收集器运行示意图：
![CMS收集器图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/f60599b2.png)
其缺点是：

- 对CPU资源敏感
- 对于在标记过程中新出现的“浮动垃圾”对象，需要等待下一次GC再清理，因此需要预留空间提供程序使用。默认当老年代使用了68%的空间后就被激活。当CMS运行期间预留内存无法满足程序需要，则会触发Concurrent Mode Failure，临时启动Serial Old收集器来进行老年代收集，导致过长停顿时间。
- 产生空间碎片，无法为大对象分配足够空间，而提前触发Full GC，同样会导致过长停顿时间。

### 2.6.7 G1
G1（Garbage-First）收集器首先将堆分为大小相等的 Region，避免全区域的垃圾收集，然后追踪每个 Region 垃圾堆积的价值大小，在后台维护一个优先列表，根据允许的收集时间优先回收价值最大的 Region；同时 G1 GC 采用 Remembered Set 来存放 Region 之间的对象引用以及其他回收器中的新生代与老年代之间的对象引用，从而避免全堆扫描。G1分区示例如下所示：
![G1分区图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/0bce1667.png)
一个region有可能属于Eden，Survivor或者Tenured内存区域。图中的E表示该region属于Eden内存区域，S表示属于Survivor内存区域，T表示属于Tenured内存区域。图中空白的表示未使用的内存空间。G1垃圾收集器还增加了一种新的内存区域，叫做Humongous内存区域，如图中的H块。这种内存区域主要用于存储大对象-即大小超过一个region大小的50%的对象。
总结而言，G1收集器的特性如下：

- 并行与并发：G1能充分利用多核环境，用多个CPU来缩短Stop-The-World停顿时间。部分其他收集器需要停顿Java线程执行的GC动作，G1可以通过并发的方式让Java程序继续执行；
- 分代收集：G1不需要其他收集器配合就可以独立管理整个堆，并采用不同的方式去处理对象以获取更好的收集效果；
- 空间整理：从整体来看，G1是基于标记-整理算法实现的，从两个Region的局部来看是基于复制算法实现的；这意味着G1算法不会产生内存空间碎片；
- 可预见性：G1 建立可预存停顿的模型，这样在用户设置的停顿时间范围内，G1 会选择适当的区域进行收集，确保停顿时间不超过用户指定时间。

G1收集器不计维护Remembered Set的整体工作步骤如下：

- 初始标记（Initial Mark）：标记GC Roots能直接关联的对象，修改TAMS（Next Top at Mark Start）值以便下一阶段用户程序并发运行时在可用Region中创建新对象，需要Stop-The-World，但耗时很短；
- 并发标记（Concurrent Mark）：从GC Root对堆中对象进行可达性分析找存活的对象，耗时较长但可以与用户线程并发执行；
- 最终标记（Remark）：为了修正并发标记期间产生变动的那一部分标记记录，期间的变化记录在Remembered Set Log里，然后合并到Remembered Set里，该阶段需要Stop-The-World，但是可并行执行；
- 筛选回收（Clean up / Copy）：对各个Region回收价值和成本排序，根据用户期望的停顿时间制定回收计划来回收。筛选回收如下图所示：
 ![G1收集器筛选回收图例](http://localhost:9000/api/file/getImage?fileId=5c8b11c7286ddd0d76000018)

### 2.6.8 回收器触发时机
- Serial Old与Parallel Old的触发是在要执行Young GC时预测其产生的即将升为老生代对象的大小超过老生代剩余大小；
- CMS初始标记的触发条件是老生代使用比率超过某值；
- G1初始标记的触发条件是Heap使用比率超过某值；
- Full GC for CMS和Full GC for G1的触发原因是CMS和G1产生诸如并发模式失败、晋升失败、大对象分配失败等情况。

### 2.6.9 总结
- Serial GC 适合最小化地使用内存和并行开销的场景；
- Parallel GC 适合最大化应用程序吞吐量的场景；
- CMS GC 适合最小化中断或停顿时间的场景。
下图显示了多种垃圾回收器之间的关系。如果两个收集器之间存在连线，则说明它们可以搭配使用。
![一文看懂JVM内存布局及GC原理](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/ab1c7be2fa4b5d180ffa1f43bf9dfce7-20200610170116481.png)

----------

# 3 Java类加载机制
## 3.1 概述
Class文件由类装载器装载后，在JVM中将形成一份描述Class结构的元信息对象，通过该元信息对象可以获知Class的结构信息：如构造函数，属性和方法等，Java允许用户借由这个Class相关的元信息对象间接调用Class对象的功能。
虚拟机把描述类的数据从class文件加载到内存，并对数据进行校验，转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型，这就是虚拟机的**类加载机制**。
在Java语言中，类型的加载、连接和初始化都是在程序运行期间完成的。这为Java提供了高度灵活性，其天生可以动态扩展的语言特性就是依赖运行期动态加载和动态连接实现的。

## 3.2 类加载时机
类从被加载到虚拟机内存中开始，到卸载出内存为止，它的整个**生命周期**包括：Loading（加载）、Verification（验证）、Preparation（准备）、Resolution（解析）、Initialization（初始化）、Using（使用）和Unloading（卸载）7个阶段。其中验证、准备、解析3个部分统称为连接（Linking）。如下图所示：
![类的生命周期图例](http://localhost:9000/api/file/getImage?fileId=5c8cbd9b286ddd0d7600001b)
加载→验证→准备→初始化→卸载，这五个阶段是按顺序进行的。但是解析阶段在某些情况下可以在初始化阶段之后执行，这是为了支持Java语言的运行时绑定（动态绑定/晚期绑定）。这几个阶段按顺序**开始**（不是进行或完成），通常是互相交叉的混合式进行，会在一个阶段执行的过程中调用、激活另一个阶段。
Java虚拟机规范严格规定了有且仅有的5种必须立即对类进行**初始化**的情况（即对一个类进行**主动引用**）：

- 遇到new、getstatic、putstatic或invokestatic这四条字节码指令时，如果类没有进行过初始化，就需要进行类的初始化；常见场景有：使用new关键字实例化对象时、读取或设置一个类的静态字段（被final修饰、已在编译器把结果放入常量池的静态字段除外）时，以及调用一个类的静态方法时；
- 使用java.lang.reflect包的方法对类进行反射调用的时候，如果类没有进行过初始化，就需要先进行类的初始化；
- 当初始化一个类的时候，如果发现父类还没有进行过初始化，则需要先初始化父类；
- 当虚拟机启动时，用户需要指定一个要执行的主类（包含main方法的那个类），虚拟机会先初始化这个主类；
- 当使用JDK 1.7的动态语言支持时，如果一个java.lang.invoke.MethodHandle实例最后的解析结果REF_getStatic、REF_putStatic、REF_invokeStatic的方法句柄，并且这个方法句柄所对应的类没有进行过初始化，则需要先触发其初始化。

## 3.3 类加载的过程
![类加载过程图例](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/20200610170157.png)

### 3.3.1 Loading - 加载
在加载阶段，虚拟机需要完成以下3件事情：

- 通过一个类的全限定名来获取此类的二进制字节流；
- 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构；
- 在内存中生成一个代表这个类的java.lang.Class对象,作为方法区这个类的各种数据的访问入口。

获取一个类的class文件可以通过多种方法实现，如：从ZIP包中读取（JAR格式）、从网络传输中获取（Applet）、运行时计算生成（动态代理技术）、由其他文件生成（JSP）、从数据库中读取等。
类加载过程中，非数组类的加载阶段是开发人员可控性最强的，既可以使用系统提供的引导类加载器来完成，也可以由用户自定义的类加载器去完成。而数组类本身不通过类加载器创建，而是由JVM直接创建，但数组元素类型由类加载器来创建。
加载阶段完成后，class类就按照格式存储在方法区中，并在内存中存在一个实例化的java.lang.Class类的对象，作为程序访问方法区中这些类型数据的外部接口。

### 3.3.2 Verification - 验证
验证作为连接阶段的第一步，目的是为了确保class文件字节流中包含的信息符合虚拟机要求，并且不会危害虚拟机自身安全。整体来看，验证阶段完成四个阶段的检验：

- 文件格式验证：对class文件的二进制流数据进行验证，是否符合文件规范（魔数，版本号，常量，常量索引等）；只有通过验证的数据才能将流数据存储在方法区中；
- 元数据验证：对字节码描述的信息进行语义分析，如是否有父类（除java.lang.Object类）、是否继承了不允许被继承的final类、非抽象类是否实现了父类或接口要求实现的所有方法、子类方法是否与父类冲突等；
- 字节码验证：通过数据流和控制流分析，对类的方法体进行验证，确认语义合法，比如跳转语句不能越界，类型转换的正确性等；即使通过了验证，也不能完全保证代码绝对安全；
- 符号引用的验证：发生在虚拟机将符号引用转换为直接引用的时候（解析阶段），即是对类自身以外的信息（常量池中的符号引用）进行匹配性的校验；如，通过全限定名能否找到对应的类，类中是否存在符合方法字段描述的方法或字段，符号引用的类方法或字段的访问性是否可以被当前类访问等；目的是确保解析动作的正常执行；

### 3.3.3 Preparation - 准备
准备阶段为类的静态变量在方法区中分配内存，并将其初始化为默认值。基本数据类型的零值如下：
数据类型|零 值   |数据类型|零 值  |数据类型 |零 值   |
--------|--------|--------|-------|---------|--------|
int     |0       |long    |0L     |short    |(short)0
char    |'\u0000'|byte    |(byte)0|boolean  |false
float   |0.0f    |double  |0.0d   |reference|null

### 3.3.4 Resolution - 解析
解析是虚拟机将常量池内的符号引用替换成直接引用的过程，一般包括类或接口，字段，类方法，接口方法四种符号引用。

- 符号引用：即一个字符串，给出了能够唯一性识别一个方法，一个变量，一个类的相关信息。符号引用与虚拟机实现的内存布局无关，引用的目标不一定已经加载在内存中了；
- 直接引用：可以是直接指向目标对象的指针，相对偏移量或者是一个能间接定位到目标的句柄；与虚拟机的内存分布是相关的，引用的目标一定在内存中已经存在了；比如类方法，类变量的直接引用是指向方法区的指针；而实例方法，实例变量的直接引用则是从实例的头指针开始算起到这个实例变量位置的偏移量。

### 3.3.5 Initialization - 初始化

初始化为类的静态变量赋予正确的初始值，JVM负责对类进行初始化，主要对类变量进行初始化。在Java中对类变量进行初始值设定有两种方式：

1. 声明类变量是指定初始值
2. 使用静态代码块为类变量指定初始值

JVM初始化步骤

1. 假如这个类还没有被加载和连接，则程序先加载并连接该类
2. 假如该类的直接父类还没有被初始化，则先初始化其直接父类
3. 假如类中有初始化语句，则系统依次执行这些初始化语句

类初始化时机——只有当对类的主动使用的时候才会导致类的初始化，类的主动使用包括以下六种：

- 创建类的实例，也就是new的方式
- 访问某个类或接口的静态变量，或者对该静态变量赋值
- 调用类的静态方法
- 反射（如`Class.forName(“com.shengsiyuan.Test”)`）
- 初始化某个类的子类，则其父类也会被初始化
- Java虚拟机启动时被标明为启动类的类（`Java Test`），直接使用`java.exe`命令来运行某个主类

### 3.3.6 结束生命周期

在如下几种情况下，Java虚拟机将结束生命周期

- 执行了`System.exit()`方法
- 程序正常执行结束
- 程序在执行过程中遇到了异常或错误而异常终止
- 由于操作系统出现错误而导致Java虚拟机进程终止

## 3.4 类加载器

----------

# 4 动态绑定与静态绑定
## 4.1 绑定
将一个方法的调用与方法所在的类（方法主体）关联起来，即决定调用哪个方法与变量。动态绑定与静态绑定，又称前期绑定和后期绑定。在Java中**重载**的方法使用静态绑定，**重写**的方法使用动态绑定。

## 4.2 静态绑定
定义：在程序执行以前已经被绑定，即在编译过程中就已经知道这个方法到底是哪个类中的方法。在编译阶段就已经指明了调用方法在常量池中的符号引用，JVM运行的时候只需要进行一次常量池解析即可。
Java中只有final、static、private修饰的方法和构造方法是静态绑定的：

- private：private修饰的方法是不能被继承的，因此子类无法访问父类中private修饰的方法，只能通过父类对象来调用该方法体。因此可以说private方法和定义这个方法的类绑定在了一起。
- final：可以被子类继承，但是不能被子类重写（覆盖），所以在子类中调用的实际是父类中定义的final方法。（使用final的两个好处：防止方法被覆盖 & 关闭动态绑定）
- static：可以被子类继承，不能被子类重写（覆盖），但是可以被子类隐藏。（如果父类里有一个static方法，子类里如果没有对应的方法，那么当子类对象调用这个方法时就会使用父类中的方法；如果子类中定义了相同的方法，则会调用子类中定义的方法。但当子类对象向上类型转换为父类对象时，不论子类中有没有定义这个静态方法，该对象都会使用父类中的静态方法。因此说静态方法可以被隐藏而不能被覆盖，与子类隐藏父类中的成员变量一致。**隐藏**是指，子类对象转换成父类对象后，能够访问父类被隐藏的变量和方法，而不能访问父类被覆盖的方法）
- 构造方法：不能被子类继承。（因为子类是通过super方法调用父类的构造函数，或者是JVM自动调用父类的默认构造方法）

因此，一个方法不可被继承，或者是被继承后不能被覆盖，那么这个方法就采用的是静态绑定。

## 4.3 动态绑定
定义：在运行时期根据具体对象的类型进行绑定。
调用的方法依赖于显示参数的实际类型，并且在运行时实现动态绑定。动态绑定的过程分为以下几个环节：
1. 编译器查看对象的声明类型和方法名；
2. 编译器查看调用方法时提供的参数类型（重载解析）；
3. 采用动态绑定调用方法的时候，调用与所引用对象的实际类型最合适的类的方法。
JAVA 虚拟机调用一个类方法时（静态方法），它会基于对象引用的类型（通常在编译时可知）来选择所调用的方法。相反，当虚拟机调用一个实例方法时，它会基于对象实际的类型（只能在运行时得知）来选择所调用的方法，这就是动态绑定，是多态的一种。动态绑定为解决实际的业务问题提供了很大的灵活性。而与方法不同，在处理Java类中的成员变量（实例变量和类变量）时，并不是采用运行时绑定，而是一般意义上的静态绑定。所以在向上转型的情况下，对象的方法可以找到子类，而对象的属性（成员变量）还是父类的属性（子类对父类成员变量的隐藏）。
根据对象的声明类型(对象引用的类型)找到“合适”的方法。具体步骤如下：
1. 如果能在声明类型中匹配到方法签名完全一样(参数类型一致)的方法，那么这个方法是最合适的；
2. 在第1条不能满足的情况下，寻找可以“凑合”的方法。标准就是通过将参数类型进行自动转型之后再进行匹配。如果匹配到多个自动转型后的方法签名f(A)和f(B)，则用下面的标准来确定合适的方法：传递给f(A)方法的参数都可以传递给f(B)，则f(A)最合适。反之f(B)最合适；
3. 如果仍然在声明类型中找不到“合适”的方法，则编译阶段就无法通过。
然后，根据在堆中创建对象的实际类型找到对应的方法表，从中确定具体的方法在内存中的位置。

## 4.4 【拓展】阻止继承
一个域、静态方法或成员类型可以分别隐藏在其超类中可访问到的具有相同名字（对方法而言就是相同的方法签名)的所有域、静态方法或成员类型。隐藏一个成员将阻止其被继承。例如：
``` Java
class Base
{
    public static void f(){}
}
class Derived extends Base
{
    private static void f(){}   //hides Base. f()
}
```
# 5 引用及相关链接
第一章引用自*《Java虚拟机规范》*。

第二、三章部分引用自“[纯洁的微笑](http://www.ityouknow.com/)”博客。
