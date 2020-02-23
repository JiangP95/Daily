# LinkedList
> 1. 双向链表。也可以被当作堆栈、队列或双端队列进行操作
> 2. 实现 List 接口，可以进行队列操作。
> 3. 实现 Deque 接口，可以当作双端队列使用，在两端访问元素，提供插入，移除，检查元素的方法。

---
> 1. 顺序访问高效，随机访问低效
> 2. 重要成员
> 	a. header是双向链表的表头，表头不包含任何数据的。是双向链表所对应的类Entry的实例。
>     b. size 双向链表中的节点个数
> 3. 重要内部类Entry：
>     previous: 当前节点的上一个节点
>     next: 当前节点的下一个节点
>     element: 当前节点值
> 4. 链表实现，不存在容量不足的问题

-----
  ## getLast()
```java
	// 获取LinkedList的最后一个元素
     public E getLast()  {
        if (size==0)
             throw new NoSuchElementException();
         // 由于LinkedList是双向链表；而表头header不包含数据。
        // 因而，这里返回表头header的前一个节点所包含的数据。
          return header.previous.element;
      }
```

  ## add(E e)
```java
	// 将元素(E)添加到LinkedList中
     public boolean add(E e) {
         // 将节点(节点数据是e)添加到表头(header)之前。
         // 即，将节点添加到双向链表的末端。
         addBefore(e, header);
         return true;
     }
```

 ## remove(Object o)
```java
// 从LinkedList中删除元素(o)
     // 从链表开始查找，如存在元素(o)则删除该元素并返回true；
     // 否则，返回false。
     public boolean remove(Object o) {
         if (o==null) {
             // 若o为null的删除情况
             for (Entry<E> e = header.next; e != header; e = e.next) {
                 if (e.element==null) {
                     remove(e);
                     return true;
                }
             }
         } else {
             // 若o不为null的删除情况
             for (Entry<E> e = header.next; e != header; e = e.next) {
                 if (o.equals(e.element)) {
                     remove(e);
                     return true;
                 }
             }
         }
         return false;
     }
```

 ## addAll(Collection<? extends E> c)
```java
  // 将“集合(c)”添加到LinkedList中。
     // 实际上，是从双向链表的末尾开始，将“集合(c)”添加到双向链表中。
     public boolean addAll(Collection<? extends E> c) {
        return addAll(size, c);
     }  
```

## addAll(int index, Collection<? extends E> c)
```java
 // 从双向链表的index开始，将“集合(c)”添加到双向链表中。
 public boolean addAll(int index, Collection<? extends E> c) {
     if (index < 0 || index > size)
         throw new IndexOutOfBoundsException("Index: "+index+
                                             ", Size: "+size);
     Object[] a = c.toArray();
     // 获取集合的长度
     int numNew = a.length;
     if (numNew==0)
         return false;
     modCount++;
 
     // 设置“当前要插入节点的后一个节点”
     Entry<E> successor = (index==size ? header : entry(index));
     // 设置“当前要插入节点的前一个节点”
     Entry<E> predecessor = successor.previous;
     // 将集合(c)全部插入双向链表中
     for (int i=0; i<numNew; i++) {
         Entry<E> e = new Entry<E>((E)a[i], successor, predecessor);
         predecessor.next = e;
         predecessor = e;
     }
     successor.previous = predecessor;
 
     // 调整LinkedList的实际大小
     size += numNew;
     return true;
 }
```
## get(int index) 
```java
// 返回LinkedList指定位置的元素
public E get(int index) {
    return entry(index).element;
}
```

## set(int index, E element) 
```java
// 设置index位置对应的节点的值为element
public E set(int index, E element) {
    Entry<E> e = entry(index);
    E oldVal = e.element;
    e.element = element;
    return oldVal;
}
```

 ## add(int index, E element) 
```java
// 在index前添加节点，且节点的值为element
public void add(int index, E element) {
    addBefore(element, (index==size ? header : entry(index)));
}
```

## remove(int index)
```java
// 删除index位置的节点
public E remove(int index) {
    return remove(entry(index));
}
```

## entry(int index)
```java
// 获取双向链表中指定位置的节点
private Entry<E> entry(int index) {
    if (index < 0 || index >= size)
        throw new IndexOutOfBoundsException("Index: "+index+
                                            ", Size: "+size);
    Entry<E> e = header;
    // 获取index处的节点。
    	// 加速动作
        // 若index < 双向链表长度的1/2,则从前先后查找;
        // 否则，从后向前查找。
        if (index < (size >> 1)) {
            for (int i = 0; i <= index; i++)
                e = e.next;
        } else {
            for (int i = size; i > index; i--)
                e = e.previous;
        }
        return e;
    }
```
## indexOf(Object o) 
```java
// 从前向后查找，返回“值为对象(o)的节点对应的索引”
// 不存在就返回-1
public int indexOf(Object o) {
    int index = 0;
    if (o==null) {
        for (Entry e = header.next; e != header; e = e.next) {
            if (e.element==null)
                return index;
            index++;
        }
    } else {
        for (Entry e = header.next; e != header; e = e.next) {
            if (o.equals(e.element))
                return index;
            index++;
        }
    }
    return -1;
}
```

## lastIndexOf(Object o)
```java
// 从后向前查找，返回“值为对象(o)的节点对应的索引”
// 不存在就返回-1
public int lastIndexOf(Object o) {
    int index = size;
    if (o==null) {
        for (Entry e = header.previous; e != header; e = e.previous) {
            index--;
            if (e.element==null)
                return index;
        }
    } else {
        for (Entry e = header.previous; e != header; e = e.previous) {
            index--;
            if (o.equals(e.element))
                return index;
        }
    }
    return -1;
}
```

----
## 作为FIFO先进先出队列
> 队列方法       等效方法
> add(e)        addLast(e)
> offer(e)      offerLast(e)
> remove()      removeFirst()
> poll()        pollFirst()
> element()     getFirst()
> peek()        peekFirst()

## 作为LIFO后进先出栈
> 栈方法        等效方法
> push(e)      addFirst(e)
> pop()        removeFirst()
> peek()       peekFirst()

## 遍历方式
> 遍历LinkedList时，使用removeFist()或removeLast()效率最高。但用它们遍历时，会删除原始数据；若单纯只读取，而不删除，应该使用通过for循环的变种来访问遍历LinkedList。禁止使用随机访问去遍历LinkedList。