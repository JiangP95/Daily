# 概览
![collection-overview.jpg](./.assets/collection-overview.jpg)


## 大致说明：

看上面的框架图，先抓住它的主干，即Collection和Map。 

1. Collection是一个接口，是高度抽象出来的集合，它包含了集合的基本操作和属性。
  Collection包含了List和Set两大分支。
  	+ List是一个有序的队列，每一个元素都有它的索引。第一个元素的索引值是0。
          List的实现类有LinkedList, ArrayList, Vector, Stack。
    + Set是一个不允许有重复元素的集合。
          Set的实现类有HastSet和TreeSet。HashSet依赖于HashMap，它实际上是通过	       HashMap实现的；TreeSet依赖于TreeMap，它实际上是通过TreeMap实现的。

2. Map是一个映射接口，即key-value键值对。
   + Map中的每一个元素包含“一个key”和“key对应的value”。
   + AbstractMap是个抽象类，它实现了Map接口中的大部分API。而HashMap，TreeMap，WeakHashMap都是继承于AbstractMap。
   + Hashtable虽然继承于Dictionary，但它实现了Map接口。

3. 接下来，再看Iterator。
	+ 它是遍历集合的工具，即我们通常通过Iterator迭代器来遍历集合。我们说Collection依赖于Iterator，是因为Collection的实现类都要实现iterator()函数，返回一个Iterator对象。
ListIterator是专门为遍历List而存在的。

4. 再看Enumeration
	+ 它是JDK 1.0引入的抽象类。作用和Iterator一样，也是遍历集合；但是Enumeration的功能要比Iterator少。在上面的框图中，Enumeration只能在Hashtable, Vector, Stack中使用。

5. 最后，看Arrays和Collections。它们是操作数组、集合的两个工具类。

有了上面的整体框架之后，我们接下来对每个类分别进行分析。

# 容器扩容
	初始长度/扩容倍数 
- ArrayList：10/1.5 
- ArrayDeque：8/2 
- BitSet：64/2 
- HashMap：16/2 
- HashSet/TreeSet：同HashMap(基于HashMap实现，value为空Object) 
- Hashtable：11/2 
- Vector：10/2 或者指定capacityIncrement
- WeakHashMap：同HashMap 
- PriorityQueue：11/Double size if small; else grow by 50% 
- StringBuilder：16/按需
