# 内存溢出与内存泄漏
> 1. 内存泄漏Memory leak，是指程序在申请内存后，无法释放已申请的内存空间，一次内存泄漏似乎不会有大的影响，但是内存泄漏后的后果就是内存溢出。
> 2. 内存溢出out of memory，是指程序申请内存时，没有足够的内存供内存申请者使用，此时就会报OOM

 # Java内存泄漏的根本原因：
> 长生命周期对象持有短生命周期对象的引用很可能发生内存泄漏，尽管短生命周期对象已经不再需要，但是因为长生命周期持有它的引用而导致不能被回收。

## 1 静态集合类引起内存泄漏：

> 像HashMap、Vector等的使用最容易出现内存泄露，这些静态变量的生命周期和应用程序一致，他们所引用的所有的对象Object也不能被释放，因为他们也将一直被Vector等引用着
> 例如
>
> ```java
> Static Vector v = new Vector(10);
> for (int i = 1; i<100; i++){
>     Object o = new Object();
>     v.add(o);
>     o = null;
> }
> ```
>
> 在这个例子中，循环申请Object 对象，并将所申请的对象放入一个Vector 中，如果仅仅释放引用本身（o=null），那么Vector 仍然引用该对象，所以这个对象对GC 来说是不可回收的。因此，如果对象加入到Vector 后，还必须从Vector 中删除，最简单的方法就是将Vector对象设置为null。



## 2、当集合里面的对象属性被修改后，再调用remove()方法时不起作用。
```java
public static void main(String[] args){
	Set<Person> set = new HashSet<Person>();
	Person p1 = new Person("唐僧","pwd1",25);
  		Person p2 = new Person("孙悟空","pwd2",26);
  		Person p3 = new Person("猪八戒","pwd3",27);
  		set.add(p1);
  		set.add(p2);
  		set.add(p3);
  		System.out.println("总共有:"+set.size()+" 个元素!"); //结果：总共有:3 个元素!
  		p3.setAge(2); //修改p3的年龄,此时p3元素对应的hashcode值发生改变set.remove(p3); //此时remove不掉，造成内存泄漏
  		set.add(p3); //重新添加，居然添加成功System.out.println("总共有:"+set.size()+" 个元素!"); //结果：总共有:4 个元素!
  		for (Person person : set){
  			System.out.println(person);
  		}
  }
```
  		

## 3、监听器
> 在java 编程中，我们都需要和监听器打交道，通常一个应用当中会用到很多监听器，我们会调用一个控件的诸如addXXXListener()等方法来增加监听器，但往往在释放对象的时候却没有记住去删除这些监听器，从而增加了内存泄漏的机会。

## 4、各种连接
> ​	比如数据库连接（dataSourse.getConnection()），网络连接(socket)和io连接，除非其显式的调用了其close（）方法将其连接关闭，否则是不会自动被GC 回收的。对于Resultset 和Statement 对象可以不进行显式回收，但Connection 一定要显式回收，因为Connection 在任何时候都无法自动回收，而Connection一旦回收，Resultset 和Statement 对象就会立即为NULL。但是如果使用连接池，情况就不一样了，除了要显式地关闭连接，还必须显式地关闭Resultset Statement 对象（关闭其中一个，另外一个也会关闭），否则就会造成大量的Statement 对象无法释放，从而引起内存泄漏。这种情况下一般都会在try里面去的连接，在finally里面释放连接。

## 5、内部类和外部模块的引用
> ​	内部类的引用是比较容易遗忘的一种，而且一旦没释放可能导致一系列的后继类对象没有释放。此外程序员还要小心外部模块不经意的引用，例如程序员A 负责A 模块，调用了B 模块的一个方法如：
> public void registerMsg(Object b);
> 这种调用就要非常小心了，传入了一个对象，很可能模块B就保持了对该对象的引用，这时候就需要注意模块B 是否提供相应的操作去除引用。

## 6、单例模式

> 不正确使用单例模式是引起内存泄漏的一个常见问题，单例对象在初始化后将在JVM的整个生命周期中存在（以静态变量的方式），如果单例对象持有外部的引用，那么这个对象将不能被JVM正常回收，导致内存泄漏，考虑下面的例子：
>
> ```java
> class A{
>   public A(){
>   	B.getInstance().setA(this);
>   }
> ....
> }
> //B类采用单例模式
> class B{
>   private A a;
>   private static B instance=new B();
>   public B(){}
>   public static B getInstance(){
>   	return instance;
>   }
>   public void setA(A a){
>   	this.a=a;
>   }
>   //getter...
> }
> ```
>
> 显然B采用singleton模式，它持有一个A对象的引用，而这个A类的对象将不能被回收。想象下如果A是个比较复杂的对象或者集合类型会发生什么情况。

# java内存溢出常见的有：
>第一种OutOfMemoryError： PermGen space
>发生这种问题的原意是程序中使用了大量的jar或class，使java虚拟机装载类的空间不够，与Permanent Generation space有关。解决这类问题有以下两种办法：
>增加java虚拟机中的XX:PermSize和XX:MaxPermSize参数的大小，其中XX:PermSize是初始永久保存区域大 小，XX:MaxPermSize是最大永久保存区域大小。如针对tomcat6.0，在catalina.sh 或catalina.bat文件中一系列环境变量名说明结束处（大约在70行左右） 增加一行： JAVA_OPTS=" -XX:PermSize=64M -XX:MaxPermSize=128m" 如果是windows服务器还可以在系统环境变量中设置。感觉用tomcat发布sprint+struts+hibernate架构的程序时很容易发生这种内存溢出错误。使用上述方法，我成功解决了部署ssh项目的tomcat服务器经常宕机的问题。
>清理应用程序中web-inf/lib下的jar，如果tomcat部署了多个应用，很多应用都使用了相同的jar，可以将共同的jar移到 tomcat共同的lib下，减少类的重复加载。这种方法是网上部分人推荐的，我没试过，但感觉减少不了太大的空间，最靠谱的还是第一种方法。

----
> 第二种OutOfMemoryError：  Java heap space
> 发生这种问题的原因是java虚拟机创建的对象太多，在进行垃圾回收之间，虚拟机分配的到堆内存空间已经用满了，与Heap space有关。解决这类问题有两种思路：
> 检查程序，看是否有死循环或不必要地重复创建大量对象。找到原因后，修改程序和算法。 我以前写一个使用K-Means文本聚类算法对几万条文本记录（每条记录的特征向量大约10来个）进行文本聚类时，由于程序细节上有问题，就导致了 Java heap space的内存溢出问题，后来通过修改程序得到了解决。
> 增加Java虚拟机中Xms（初始堆大小）和Xmx（最大堆大小）参数的大小。如：set JAVA_OPTS= -Xms256m -Xmx1024m
> 第三种OutOfMemoryError：unable to create new native thread
> 在java应用中，有时候会出现这样的错误：OutOfMemoryError: unable to create new native thread.这种怪事是因为JVM已经被系统分配了大量的内存(比如1.5G)，并且它至少要占用可用内存的一半。有人发现，在线程个数很多的情况下， 你分配给JVM的内存越多，那么，上述错误发生的可能性就越大。

----
> 那么是什么原因造成这种问题呢？
> 每一个32位的进程最多可以使用2G的可用内存，因为另外2G被操作系统保留。这里假设使用1.5G给JVM，那么还余下500M可用内存。这 500M内存中的一部分必须用于系统dll的加载，那么真正剩下的也许只有400M，现在关键的地方出现了：当你使用Java创建一个线程，在JVM的内 存里也会创建一个Thread对象，但是同时也会在操作系统里创建一个真正的物理线程(参考JVM规范)，操作系统会在余下的400兆内存里创建这个物理 线程，而不是在JVM的1500M的内存堆里创建。在jdk1.4里头，默认的栈大小是256KB，但是在jdk1.5里头，默认的栈大小为1M每线程， 因此，在余下400M的可用内存里边我们最多也只能创建400个可用线程。
> 这样结论就出来了，要想创建更多的线程，你必须减少分配给JVM的最大内存。还有一种做法是让JVM宿主在你的JNI代码里边。

