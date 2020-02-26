# 常用JVM工具

- ## jps

用来查看基于HotSpot的JVM里面中，所有具有访问权限的Java进程的具体状态, 包括进程ID，进程启动的路径及启动参数等等，与unix上的ps类似，但是jps仅显示Java进程。

- ## jstat

用于监控基于HotSpot的JVM，对其堆的使用情况进行实时的命令行的统计。

**常用模式**

- - - jstat -gc {进程号}：垃圾回收的行为统计
    - jstat -gccapacity {进程号}：同-gc，还会输出Java堆各区域使用到的最大、最小空间
    - jstat -gcutil {进程号}：同-gc，输出的是已使用空间占总空间的百分比
    - jstat -gccause {进程号}：垃圾收集统计概述（同-gcutil），附加最近两次垃圾回收事件的原因
    - jstat -gcnew {进程号}：统计新生代行为
    - jstat -gcold {进程号}：统计老年代行为

- ## jmap

打印出某个java进程（使用pid）内存中所有对象的情况（如：产生那些对象，及其数量）。

可以用于产生heap dump

**常用模式**

- - - jmap -dump:live,format=b,file={文件路径和文件名} {进程号}：dump heap到文件,format指定输出格式，live指明是仅包含存活的对象（注意：加上此参数会触发一次Full GC）
    - jmap -heap {进程号}：打印heap的概要信息，GC使用的算法，heap的配置及使用情况，可以用此来判断内存目前的使用情况以及垃圾回收情况
    - jmap -histo {进程号}：打印堆的对象统计，包括对象数、内存大小等等。jmap -histo:live 这个命令执行，JVM会先触发gc，然后再统计信息
    - jmap -permstat {进程号}：打印Java堆内存的永久区的类加载器的智能统计信息。对于每个类加载器而言，它的名称、活跃度、地址、父类加载器、它所加载的类的数量和大小都会被打印。此外，包含的字符串数量和大小也会被打印。

- ## jstack

jstack用于打印出给定的java进程ID的Java堆栈信息

**常用模式**

- - jstack {进程号}：打印此Java进程此时的所有运行时线程的堆栈信息