# HashMap
> 1. 散列表，键值对（Key-Value）映射
>
> 2. 线程非安全
>
> 3. Key，Value都可以为null
>
> 4. 映射不是有序的
>
> 5. 初始容量；加载因子：默认值0.75
> 	当hash表的条目数超过  加载因子*当前容量
>     时，则对hash表进行扩容操作
>    
> 6. hashMap将key为null的元素放在table[0]位置 
>
> 7. 重要属性：
> 	Entry[] table：储存键值对，Entry实际上就是一个单向链表
>     size：hashmap的大小
>     threshold：阈值，用于判断是否需要调整hashmap的容量。threshold = 当前容量*加载因子
>     loadFactor：加载因子
>     modCount：实现fail-fast机制，也是hashmap被改变的次数
>    
> 8. HashMap就是一个散列表，它是通过“拉链法”解决哈希冲突的
>
> 9. 线程不安全：
>
>    a. 多线程hash值相同时，会引起值覆盖
>
>    b. 多线程同时扩容时，会引起环装链表，get的时候则会引起死循环

 ## get(Object key)
```java
	    // 获取key对应的value
    public V get(Object key) {
        if (key == null)
            return getForNullKey();
        // 获取key的hash值
        int hash = hash(key.hashCode());
        // 在“该hash值对应的链表”上查找“键值等于key”的元素
        for (Entry<K,V> e = table[indexFor(hash, table.length)];
             e != null;
             e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k)))
                return e.value;
        }
        return null;
    }
```

 ## put(K key, V value)
```java
 // 将“key-value”添加到HashMap中
    public V put(K key, V value) {
        // 若“key为null”，则将该键值对添加到table[0]中。
        if (key == null)
            return putForNullKey(value);
        // 若“key不为null”，则计算该key的哈希值，然后将其添加到该哈希值对应的链表中。
        int hash = hash(key.hashCode());
        int i = indexFor(hash, table.length);
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            // 若“该key”对应的键值对已经存在，则用新的value取代旧的value。然后退出！
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
        // 若“该key”对应的键值对不存在，则将“key-value”添加到table中
        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }
```

## hash(int h)
```java
  static int hash(int h) {
      h ^= (h >>> 20) ^ (h >>> 12);
      return h ^ (h >>> 7) ^ (h >>> 4);
  }
```

 ## indexFor(int h, int length)
```java
  // 返回索引值
  // h & (length-1)保证返回值的小于length
  static int indexFor(int h, int length) {
      return h & (length-1);
  }
```

## addEntry(int hash, K key, V value, int bucketIndex)
```java
 // 用在 新增Entry可能导致“HashMap的实际容量”超过“阈值” 的情况 如：put
 // 新增Entry。将“key-value”插入指定位置，bucketIndex是位置索引。
 void addEntry(int hash, K key, V value, int bucketIndex) {
     // 保存“bucketIndex”位置的值到“e”中
     Entry<K,V> e = table[bucketIndex];
     // 设置“bucketIndex”位置的元素为“新Entry”，
     // 设置“e”为“新Entry的下一个节点”
     table[bucketIndex] = new Entry<K,V>(hash, key, value, e);
     // 若HashMap的实际大小 不小于 “阈值”，则调整HashMap的大小
     if (size++ >= threshold)
         resize(2 * table.length);
 }
```
## createEntry(int hash, K key, V value, int bucketIndex)
```java
// 用在 新增Entry不会导致“HashMap的实际容量”超过“阈值”的情况，如构造方法中
void createEntry(int hash, K key, V value, int bucketIndex) {
    // 保存“bucketIndex”位置的值到“e”中
    Entry<K,V> e = table[bucketIndex];
    // 设置“bucketIndex”位置的元素为“新Entry”，
    // 设置“e”为“新Entry的下一个节点”
    table[bucketIndex] = new Entry<K,V>(hash, key, value, e);
    size++;
}
```

## putForNullKey()
```java
 // putForNullKey()的作用是将“key为null”键值对添加到table[0]位置
private V putForNullKey(V value) {
    for (Entry<K,V> e = table[0]; e != null; e = e.next) {
        if (e.key == null) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
    // 这里的完全不会被执行到!
    modCount++;
    addEntry(0, null, value, 0);
    return null;
}
```

## resize(int newCapacity)
```java
// 重新调整HashMap的大小，newCapacity是调整后的单位
    void resize(int newCapacity) {
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }
        // 新建一个HashMap，将“旧HashMap”的全部元素添加到“新HashMap”中，
        // 然后，将“新HashMap”赋值给“旧HashMap”。
        Entry[] newTable = new Entry[newCapacity];
        transfer(newTable);
        table = newTable;
        threshold = (int)(newCapacity * loadFactor);
    }
```

## transfer(Entry[] newTable) 
```java
// 将HashMap中的全部元素都添加到newTable中
void transfer(Entry[] newTable) {
    Entry[] src = table;
    int newCapacity = newTable.length;
    for (int j = 0; j < src.length; j++) {
        Entry<K,V> e = src[j];
        if (e != null) {
            src[j] = null;
            do {
                Entry<K,V> next = e.next;
                int i = indexFor(e.hash, newCapacity);
                e.next = newTable[i];
                newTable[i] = e;
                e = next;
            } while (e != null);
        }
    }
}
```