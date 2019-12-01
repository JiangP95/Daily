# TreeSet

1. 有序的Set集合

2. 基于TreeMap实现的

3. 非线程安全

4. 重要成员：

   NavigableMap<E,Object> m = new TreeMap<E,Object>()



## add(E e)

​	 // 添加e到TreeSet中 

​	public boolean add(E e) { 

​		return m.put(e, PRESENT)==null; 

} 

## remove(Object o) 

​	// 删除TreeSet中的对象o 

​	public boolean remove(Object o) { 

​		return m.remove(o)==PRESENT; 

​	}