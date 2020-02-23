# ArrayList
```java
1. 数组队列,  相当于 动态数组
2. 实现了RandmoAccess接口，即提供了随机访问功能。RandmoAccess是java中用来被List实现，为List提供快速访问功能的。可以通过元素的序号快速获取元素对象；这就是快速随机访问
3. 非线程安全的！
4. 初始默认容量/扩容倍数：10/1.5
5. 重要对象
	elementData: Object[] ，保存添加到ArrayList中的元素
    size: 动态数组大小
```
---
## ensureCapacity(int minCapacity)
```java
// 确定ArrarList的容量。
// 若ArrayList的容量不足以容纳当前的全部元素，设置 新的容量=“(原始容量x3)/2 + 1”
public void ensureCapacity(int minCapacity) {
    // 将“修改统计数”+1
    modCount++;
    int oldCapacity = elementData.length;
    // 若当前容量不足以容纳当前的元素个数，设置 新的容量=“(原始容量x3)/2 + 1”
    if (minCapacity > oldCapacity) {
       Object oldData[] = elementData;
       int newCapacity = (oldCapacity * 3)/2 + 1;
       if (newCapacity < minCapacity)
           newCapacity = minCapacity;
          elementData = Arrays.copyOf(elementData, newCapacity);
    }
 }
```

## add(E e)
```java
// 添加元素e
       public boolean add(E e) {
           // 确定ArrayList的容量大小,不够则扩容
           ensureCapacity(size + 1);  // Increments modCount!!
           // 添加e到ArrayList中
           elementData[size++] = e;
           return true;
      }
```
## add(int index, E e)
```java
// 将指定的元素插入此列表中的指定位置。
// 如果当前位置有元素，则向右移动当前位于该位置的元素以及所有后续元素（将其索引加 1）。
 public void add(int index, E element) {
       if (index > size ||index < 0)
 			throw new IndexOutOfBoundsException("Index:"+index+",Size:"+siz e);
     // 如果数组长度不足，将进行扩容。
     ensureCapacity(size+1);  // IncrementsmodCount!!
     // 将 elementData中从 Index位置开始、长度为size-index的元素，
     // 拷贝到从下标为index+1位置开始的新的elementData数组中。
     // 即将当前位于该位置的元素以及所有后续元素右移一个位置。
	System.arraycopy(elementData,index,elementData,index+1,size-index);
    elementData[index] = element;
    size++;
    }
```

 ## remove(int index)
```java
// 删除ArrayList指定位置的元素
    public E remove(int index) {
         RangeCheck(index);
         modCount++;
         E oldValue = (E) elementData[index];
         // 被删除元素后面的个数
         int numMoved = size - index - 1;
         // 后面所有的元素用copy的方式前移
         if (numMoved > 0)
             System.arraycopy(elementData, index+1, elementData, index,
                  numMoved);
         elementData[--size] = null; // Let gc do its work
         return oldValue;
     }
```

​        
 ## remove(Object o)
```java
 // 删除ArrayList的指定元素，遍历移除
    public boolean remove(Object o) {
        if (o == null) {
                 for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                 fastRemove(index);
                return true;
            }
        } else {
            for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
               fastRemove(index);
                return true;
            }
        }
         return false;
     }
```
  ## fastRemove(int index)
```java
  // 快速删除第index个元素
     private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
         // 从"index+1"开始，用后面的元素替换前面的元素。
        if (numMoved > 0)
             System.arraycopy(elementData, index+1, elementData, index,
                              numMoved);
         // 将最后一个元素设为null
        elementData[--size] = null; // Let gc do its work
     }
```