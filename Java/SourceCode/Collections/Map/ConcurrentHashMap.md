#  ConcurrentHashMap

**jdk1.8之前**

![concurrentHashMap-1.png](../img/concurrentHashMap-1.png)



> ConcurrentHashMap结构分为两部分：**segment数组，一旦初始化以后，不可扩容**；segment中的内部数组和链表，**内部数组可扩容**
>
> concurrencyLevel： 并行级别、并发数、Segment 数，默认值是16，也就是说 ConcurrentHashMap 有 16 个 Segments，所以理论上，这个时候，最多可以同时支持 16 个线程并发写，只要它们的操作分别分布在不同的 Segment 上。

**jdk1.8之后**

![concurrentHashMap-1.png](../img/concurrentHashMap-2.png)

>JDK1.8之后ConcurrentHashMap结构和JDK1.8之后的HashMap基本上一样，也是保持着数组+链表+红黑树的结构，不同的是，ConcurrentHashMap需要保证线程安全性
>
>数组长度超过8之后，转为红黑树结构   
>
>数据查询复杂的 O(n)，红黑树查询时间复杂度 O(logN)

```java

	/*
     * 
     *  当设置为false的时候表示这个value一定会设置
     *  true的时候，只有当这个key的value为空的时候才会设置
     */
    public V put(K key, V value) {
        return putVal(key, value, false);
    }

/*
     * 当添加一对键值对的时候，首先会去判断保存这些键值对的数组是不是初始化了，
     * 如果没有的话就初始化数组
     *  然后通过计算hash值来确定放在数组的哪个位置
     * 如果这个位置为空则直接添加，如果不为空的话，则取出这个节点来
     * 如果取出来的节点的hash值是MOVED(-1)的话，则表示当前正在对这个数组进行扩容，复制到新的数组，则当前线程也去帮助复制
     * 最后一种情况就是，如果这个节点，不为空，也不在扩容，则通过synchronized来加锁，进行添加操作
     *    然后判断当前取出的节点位置存放的是链表还是树
     *    如果是链表的话，则遍历整个链表，直到取出来的节点的key来个要放的key进行比较，如果key相等，并且key的hash值也相等的话，
     *          则说明是同一个key，则覆盖掉value，否则的话则添加到链表的末尾
     *    如果是树的话，则调用putTreeVal方法把这个元素添加到树中去
     *  最后在添加完成之后，会判断在该节点处共有多少个节点（注意是添加前的个数），如果达到8个以上了的话，
     *  则调用treeifyBin方法来尝试将处的链表转为树，或者扩容数组
     */
    final V putVal(K key, V value, boolean onlyIfAbsent) {
        //K,V都不能为空，否则的话跑出异常
        if (key == null || value == null) throw new NullPointerException();
        //取得key的hash值
        int hash = spread(key.hashCode());    
        //用来计算在这个节点总共有多少个元素，用来控制扩容或者转移为树
        int binCount = 0;    
        for (Node<K,V>[] tab = table;;) {    //
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)    
                //第一次put的时候table没有初始化，则初始化table
                tab = initTable();    
            //通过哈希计算出一个表中的位置因为n是数组的长度，所以(n-1)&hash肯定不会出现数组越界
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {   
                 //如果这个位置没有元素的话，则通过cas的方式尝试添加，注意这个时候是没有加锁的
                //创建一个Node添加到数组中区，null表示的是下一个节点为空
                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value, null)))        						// no lock when adding to empty bin
                    break;                   
            }
            /*
             * 如果检测到某个节点的hash值是MOVED，则表示正在进行数组扩张的数据复制阶段，
             * 则当前线程也会参与去复制，通过允许多线程复制的功能，一次来减少数组的复制所带来的性能损失
             */
            else if ((fh = f.hash) == MOVED)    
                tab = helpTransfer(tab, f);
            else {
                /*
                 * 如果在这个位置有元素的话，就采用synchronized的方式加锁，
                 *     如果是链表的话(hash大于0)，就对这个链表的所有元素进行遍历，
                 *         如果找到了key和key的hash值都一样的节点，则把它的值替换到
                 *         如果没找到的话，则添加在链表的最后面
                 *  否则，是树的话，则调用putTreeVal方法添加到树中去
                 *  
                 *  在添加完之后，会对该节点上关联的的数目进行判断，
                 *  如果在8个以上的话，则会调用treeifyBin方法，来尝试转化为树，或者是扩容
                 */
                V oldVal = null;
                synchronized (f) {
                    //再次取出要存储的位置的元素，跟前面取出来的比较
                    if (tabAt(tab, i) == f) {        
                        //取出来的元素的hash值大于0，当转换为树之后，hash值为-2
                        if (fh >= 0) {                
                            binCount = 1;            
                            //遍历这个链表
                            for (Node<K,V> e = f;; ++binCount) {    
                                K ek;
                                //要存的元素的hash，key跟要存储的位置的节点的相同的时候，替换掉该节点的value即可
                                if (e.hash == hash &&        
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    //当使用putIfAbsent的时候，只有在这个key没有设置值得时候才设置
                                    if (!onlyIfAbsent)        
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                //如果不是同样的hash，同样的key的时候，则判断该节点的下一个节点是否为空，
                                if ((e = e.next) == null) {    
                                    //为空的话把这个要加入的节点设置为当前节点的下一个节点
                                    pred.next = new Node<K,V>(hash, key,value, null);
                                    break;
                                }
                            }
                        }
                         //表示已经转化成红黑树类型了
                        else if (f instanceof TreeBin) {   
                            Node<K,V> p;
                            binCount = 2;
                            //调用putTreeVal方法，将该元素添加到树中去
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                if (binCount != 0) {
                    //当在同一个节点的数目达到8个的时候，则扩张数组或将给节点的数据转为tree
                    if (binCount >= TREEIFY_THRESHOLD)    
                        treeifyBin(tab, i);    
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);    //计数
        return null;
    }
```

