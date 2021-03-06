
![JVM.png](./img/JVM.png)

# Java内存模型
> 1. 程序计数器：当前线程所执行的字节码的行号指示器
> 2. 栈区：基本数据类型，对象的引用
> 3. 堆区：对象实例，数组，（所有线程共享）
> 4. 方法区：类信息，常量，静态变量



## 程序计数器:

> 1.  线程执行的字节码行号
>       		如果执行的是java方法，记录正在执行的虚拟机字节码地址
>       		如果执行的是native方法，计数值为空
> 2. 此内存区域是唯一一个java虚拟机中没有规定任何OutOfMemoryError情况的区域



## 栈区

#### 虚拟机栈：

> 1. 每个方法执行的时候都会创建一个栈帧，用于存放局部变量表，操作数栈，动态链接，返回地址。每个方法从调用到执行完成的过程，就对应一个栈帧在虚拟机中入栈到出栈的过程
> 2. 局部变量表：
>    	存放编译期可知的各种基本数据类型和返回类型，需要注意的是其中64位长度的long和double类型的数据类型会占用2个局部变量空间（slot），其余的数据类型只占一个。局部变量表所需的内存空间在编译期间完成分配，在方法运行期间不会改变局部变量表的大小
> 3. java虚拟机栈有两种异常
>        StackOverflowError 超过栈深度
>        OutOfMemoryError 动态扩展内存不足

#### 本地方法栈：

> 1. 与java虚拟机栈类似，区别是java虚拟机栈之心java方法，本地方法栈执行本地方法。有的虚拟机会把这个栈合二为一。
>
> 2. 本地方法栈合虚拟机栈一样会出现 StackOverflowError 和 OutOfMemoryError 异常



## 堆：

> 1. 对象实例
> 2. 所有线程共享



## 方法区：

> 1. 存储已被java虚拟机加载的类信息，常量，静态变量，编译器编译后的代码
> 2. Java虚拟机规范中把方法区描述为堆的一个逻辑部分，但是他还有个别名叫 Non-Heap（非堆），目的是与Java堆区分开来

#### 运行时常量池：

> 1. 属于方法区的一部分
> 2. Class文件中除了有类的版本，字段，方法，接口等描述信息外，还有一项信息是常量池，用户存放编译器生成的各种字面量和符号引用，这部分内容将在类加载后进入方法去的运行时常量池中存放



## 直接内存：

> Deirect Memory并不是虚拟机运行时数据区的一部分，也不是Java虚拟机规范中定义的内存区域，但是这部分内存也被频繁使用，并且也可能导致OutOfMemmeryError异常，所以需要注意。服务器管理员在配置虚拟机参数时，会根据实际内存设置-Xmx等参数信息，但是经常忽略直接内存，使得各个内存区域总和大于物理内存限制（包括物理的和操作系统级的限制）从而导致动态扩展时出现OutOfMemoryError异常。






















