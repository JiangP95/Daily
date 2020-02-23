# Hashtable
> 1. 是一个散列表，键值对（key-value）映射
> 2. 所有的函数都是同步的，线程安全
> 3. key value都不可以为null
> 4. 默认加载因子为0.75
> 5. 重要属性
>    Entry[] table; 键值对，实际是一个单向链表
>    count; 数量大小
>    threshold; 阈值，用于判断是否需要调整Hashtable的容量。threshold = 容量*加载因子
>    loadFactor; 加载因子
>    modCount; 用来实现fail-fast机制

## 函数说明
> Hashtable的实现方法基本和HashMap原理一致，区别在于Hashtable的方法都用了synchronized修饰，都是同步的，即线程安全。