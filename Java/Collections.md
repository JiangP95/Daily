# 容器扩容
	初始长度/扩容倍数 
- ArrayList：10/1.5 
- ArrayDeque：8/2 
- BitSet：64/2 
- HashMap：16/2 
- HashSet/TreeSet：同HashMap(基于HashMap实现，value为空Object) 
- Hashtable：11/2 
- WeakHashMap：同HashMap 
- PriorityQueue：11/Double size if small; else grow by 50% 
- StringBuilder：16/按需