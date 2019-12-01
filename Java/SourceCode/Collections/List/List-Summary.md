# List总结
## 概览
![list.jpg](../.assets/list.jpg)


## 总结
	ArrayList,LinkedList,Vector,Stack是List的4个实现类
	1. ArrayList：数组队列，动态数组。数组实现，随机访问效率高，随机插入，随机删除效率低。线程非安全。
	2. LinkedList：双向链表。可以被当做堆栈，队列活双端队列使用。随机访问效率低，但是随机插入，删除效率高。线程非安全
	3. Vector：矢量队列，动态数组。线程安全
	4. Stack：栈，继承于Vector。先进后出。线程安全

## 使用场景
	1. 对于需要快速插入，删除元素，应该使用LinkedList。
	2. 对于需要快速随机访问元素，应该使用ArrayList。
	3. 对于“单线程环境” 或者 “多线程环境，但List仅仅只会被单个线程操作”，此时应该使用非同步的类(如ArrayList)。对于“多线程环境，且List可能同时被多个线程操作”，此时，应该使用同步的类(如Vector)
