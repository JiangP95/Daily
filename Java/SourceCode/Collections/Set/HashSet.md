# HashSet

1. 没有重复元素的集合, 不保证元素的顺序, 允许使用 null 元素.内部是通过HashMap实现的

2. 非线程安全

3. 重要成员：

   HashMap<E,Object> map；通过HashMap对象保存内容，操作都是HashMap实现的

   Object PRESENT;   PRESENT是向map中插入key-value对应的value。因为HashSet中只需要用到key，而HashMap是key-value键值对；所以，向map中添加键值对时，键值对的值固定是PRESENT

## add(E e) ：

​	// 将元素(e)添加到HashSet中     

​	public boolean add(E e) {          

​		return map.put(e, PRESENT)==null;     

​	}



## remove(Object o) 

​    // 删除HashSet中的元素(o)    

​	public boolean remove(Object o) {         

​		return map.remove(o)==PRESENT;   

​	}