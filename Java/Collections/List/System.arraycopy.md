# System.arraycopy
	JNI函数。
	在网上查到一些关于Java优化的资料里也推荐使用System.arraycopy来批量处理数组，其本质就是让处理器利用一条指令处理一个数组中的多条记录，有点像汇编语言里面的串操作指令（LODSB，LODSW，LODSB，STOSB，STOSW，STOSB），只需指定头指针然后就开始循环即可，执行一次指令，指针就后移一个位置。要操作多少个数据就循环多少次即可
---
    arraycopy方法是一个本地方法。
	在OpenJDK源码包中可以找到openjdk6-src/hotspot/src/share/vm/prims/jvm.cpp文件