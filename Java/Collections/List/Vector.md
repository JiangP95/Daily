# Vector
	1. 继承了AbstractList，实现了List，是一个队列
	2. 实现了RandmoAccess接口，支持随机访问功能
	3. 线程安全
	4. 重要对象：
		elementData:Object[]
        capacityIncrement:容量增长系数。初始化时，若指定了capacityIncrement的大小，则每次动态数组容量增加的大小都是capacityIncrement
        elementCount: 数组大小
 
 ## ensureCapacity (int minCapacity)
 	// 确定Vector的容量。容量增长
    public synchronized void ensureCapacity(int minCapacity) {
        // 将Vector的改变统计数+1
        modCount++;
        ensureCapacityHelper(minCapacity);
    }

## ensureCapacityHelper(int minCapacity)
	 // 确认“Vector容量”的帮助函数
    private void ensureCapacityHelper(int minCapacity) {
        int oldCapacity = elementData.length;
        // 当Vector的容量不足以容纳当前的全部元素，增加容量大小。
        // 若 容量增量系数>0(即capacityIncrement>0)，则将容量增大当capacityIncrement
        // 否则，将容量增大一倍。
        if (minCapacity > oldCapacity) {
            Object[] oldData = elementData;
            int newCapacity = (capacityIncrement > 0) ?
                (oldCapacity + capacityIncrement) : (oldCapacity * 2);
            if (newCapacity < minCapacity) {
                newCapacity = minCapacity;
            }
            elementData = Arrays.copyOf(elementData, newCapacity);
        }
    }