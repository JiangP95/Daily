# WeakHashMap

1. 散列表，键值对(key-value)映射，键和值都可以是null，键是“弱键”。

2. 当“弱键” 不再被其它对象引用，并被GC回收时，“弱键”也同时会被添加到ReferenceQueue(queue)队列中。当下一次我们需要操作WeakHashMap时，会先同步table和queue。table中保存了全部的键值对，而queue中保存被GC回收的键值对；同步它们，就是删除table中被GC回收的键值对。

3. 非线程安全，可以使用 Collections.synchronizedMap 方法来构造同步的 WeakHashMap

4. 重要成员变量：

   Entry[] table; Entry实际上就是一个单向链表。Entry用于存储"key-value键值。 

　　size；table的大小。 

　　threshold; 阈值，用于判断是否需要调整容量。threshold的值="容量*加载因子"。

　　loadFactor; 加载因子。 

　　modCount是用来实现fail-fast机制的

　　queue; 保存的是“已被GC清除”的“弱引用的键”。

5. 源码和 HashMap基本一致

6. “动态回收”的步骤

   (01) 新建WeakHashMap，将“键值对”添加到WeakHashMap中。

   ​    将“键值对”添加到WeakHashMap中时，添加的键都是弱键。

   ​    实际上，WeakHashMap是通过数组table保存Entry(键值对)；每一个Entry实际上是一个单向链表，即Entry是键值对链表。

   (02) 当某“弱键”不再被其它对象引用，并被GC回收时。在GC回收该“弱键”时，这个“弱键”也同时会被添加到queue队列中。

   ​    	例如，当我们在将“弱键”key添加到WeakHashMap之后；后来将key设为null。这时，便没有外部外部对象再引用该了key。

   ​    接着，当Java虚拟机的GC回收内存时，会回收key的相关内存；同时，将key添加到queue队列中。

   (03) 当下一次我们需要操作WeakHashMap时，会先同步table和queue。table中保存了全部的键值对，而queue中保存被GC回收的“弱键”；同步它们，就是删除table中被GC回收的“弱键”对应的键值对。

   ​    例如，当我们“读取WeakHashMap中的元素或获取WeakReference的大小时”，它会先同步table和queue，目的是“删除table中被GC回收的‘弱键’对应的键值对”。删除的方法就是逐个比较“table中元素的‘键’和queue中的‘键’”，若它们相当，则删除“table中的该键值对”。

   ## expungeStaleEntries()

   ​		// 清空table中无用键值对。原理如下： 

   ​		// (01) 当WeakHashMap中某个“弱引用的key”由于没有再被引用而被GC收回时， 

   ​		//   被回收的“该弱引用key”也被会被添加到"ReferenceQueue(queue)"中。 

   ​		// (02) 当我们执行expungeStaleEntries时， 

   ​		//   就遍历"ReferenceQueue(queue)"中的所有key 

   ​		//   然后就在“WeakReference的table”中删除与“ReferenceQueue(queue)中key”对应的键值对 

   ​		private void expungeStaleEntries() { 

   ​		         Entry<K,V> e; 

   ​				 while ( (e = (Entry<K,V>) 

   ​						queue.poll()) != null) { 

   ​						int h = e.hash; 

   ​						int i = indexFor(h, table.length); 

   ​						Entry<K,V> prev = table[i]; 

   ​						Entry<K,V> p = prev; 

   ​						while (p != null) { 

   ​							Entry<K,V> next = p.next; 

   ​							if (p == e) { 

   ​								if (prev == e) 

   ​									table[i] = next; 

   ​								else 

   ​									prev.next = next; 

   ​								e.next = null;  // Help GC 

   ​								e.value = null; //  "   " 

   ​								size--; 

   ​								break; 

   ​							} 

   ​						prev = p; 

   ​						p = next; 

   ​				} 

   ​		} 

   ​	}