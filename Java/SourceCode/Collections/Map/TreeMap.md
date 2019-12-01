# TreeMap
	1. 有序的key-value集合，本质是R-B Tree(红黑树)。该映射根据其键的自然顺序进行排序，或者根据创建映射时提供的 Comparator 进行排序，具体取决于使用的构造方法。
	2. 实现了NavigableMap接口，意味着它支持一系列的导航方法。比如返回有序的key集合
	3. 非线程安全
	4. 重要成员变量：
		Entry<K,V> root; root是红黑数的根节点。Entry是红黑数的节点，它包含了红黑数的6个基本组成成分：key(键)、value(值)、left(左孩子)、right(右孩子)、parent(父节点)、color(颜色)。Entry节点根据key进行排序，Entry节点包含的内容为value
	  comparator; 红黑数排序时，根据Entry中的key进行排序；Entry中的key比较大小是根据比较器comparator来进行判断的。
	  size; 是红黑数中节点的个数