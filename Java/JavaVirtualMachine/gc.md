## 对象已死？

> **可达性分析算法**
> 从GCRoots作为对象的起始点，向下搜索，当一个对象到GCRoots没有任务引用链相连时，
> 则该对象可回收。

## JVM 年轻代到年老代的晋升过程的判断条件是什么

> 虚拟机给每个对象定义一个对象年龄计数器。如果对象在Eden出生并经过第一次Minor GC后仍然存活，并且能被Survivor容纳的话，将被移动到Survivor空间中，并且对象年龄设为1。对象在Survivor区中每“熬过”一次Minor GC，年龄就增加1岁，当它的年龄增加到一定程度（默认为15岁），就将会被晋升到老年代中。对象晋升老年代的年龄阈值，可以通过参数-XX:MaxTenuringThreshold设置。
>
> 还有一种方式是动态对象年龄判定。为了适应不同程序的内存状况，虚拟机并不是永远地要求对象的年龄必须达到看MaxTenuringThreshold才能晋升老年代，如果Survivor空间中相同年龄所有对象大小的总和大于Survivor空间的一半，年龄大于或等于该年龄的对象就可以直接进入老年代，无须等到MaxTenuringThreshold中要求的年龄。

# 垃圾回收算法
## 1. 标记-清除算法
> a. 标记出所有需要回收的对象
> b. 回收被标记的对象
> 缺点：
> a. 标记和清除过程效率都不高
> b. 会产生大量不连续的内存碎片

## 2. 复制算法
> a. 将可用内存按容量划分为大小相等的两块，每次使用一块
> b. 将还存活的对象复制到另一块上面
> 缺点：内存缩小为一半

## 3. 标记-整理算法
> a. 标记出所有需要回收的对象
> b. 把所有存活的对象向一端移动

## 4. 分代收集算法
> a. 将对象分为新生代，老生代
> b. 新生代在每次回收时都有大量对象死去，只有少量存活，用复制算法，只需要复制少量对象就可以完成收集。老生代，对象存活率高，用标记清除或标记整理算法。

# gc 哪些是gcroots

> GC管理的主要区域是Java堆，一般情况下只针对堆进行垃圾回收。方法区、栈和本地方法区不被GC所管理,因而选择这些区域内的对象作为GC roots,被GC roots引用的对象不被GC回收。
>
> 一个对象可以属于多个root，GC Roots有以下几种：
>
> - Class - 由系统类加载器(system class loader)加载的对象，这些类是不能够被回收的，他们可以以静态字段的方式保存持有其它对象。我们需要注意的一点就是，通过用户自定义的类加载器加载的类，除非相应的Java.lang.Class实例以其它的某种（或多种）方式成为roots，否则它们并不是roots，.
> - Thread - 活着的线程
> - Stack Local - Java方法的local变量或参数
> - JNI Local - JNI方法的local变量或参数
> - JNI Global - 全局JNI引用
> - Monitor Used - 用于同步的监控对象
> - Held by JVM - 用于JVM特殊目的由GC保留的对象，但实际上这个与JVM的实现是有关的。可能已知的一些类型是：系统类加载器、一些JVM知道的重要的异常类、一些用于处理异常的预分配对象以及一些自定义的类加载器等。然而，JVM并没有为这些对象提供其它的信息，因此需要去确定哪些是属于"JVM持有"的了。