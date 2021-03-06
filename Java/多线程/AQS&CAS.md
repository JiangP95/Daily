# 悲观锁(独占锁):

假设一定会发生冲突，获取对象锁之后，会阻塞其他等待线程。悲观锁的代表是 synchronized

# 乐观锁：

假设一定不会发生冲突，先尝试执行某项操作，失败了再进行其他处理(一般都是不断循环重试)，不会阻塞其他线程，也不涉及上下文切换，性能开销小。乐观锁代表是 CAS

# 公平锁：

线程在获取对象锁之前，先检测有无排队的线程，按排队顺序获取锁。

# 非公平锁：

 线程在获取对象锁时，直接尝试过去锁，获取不到再去队尾排队。

# 可重入锁：

一个线程获取到的对象锁后，可以在后续重复访问这个对象。

# 不可重入锁：

一个线程获取到对象锁后，必须先释放当前的锁，才能继续访问。

# 类锁与对象锁：

synchronized 修饰静态方法时，锁的是类对象，所有线程访问这个这个静态方法都会同步顺序执行。

synchronized 修饰实例方法时，锁的是实例对象，多个线程是可以同时执行同一个synchronized实例方法，只要它们访问的对象是不同的。

# CAS(Compare And Swap)

## 什么是CAS

> CAS(Compare And Swap)，即比较并交换。是解决多线程并行情况下使用锁造成性能损耗的一种机制，CAS操作包含三个操作数——内存位置（V）、预期原值（A）和新值(B)。如果内存位置的值与预期原值相匹配，那么处理器会自动将该位置值更新为新值。否则，处理器不做任何操作。无论哪种情况，它都会在CAS指令之前返回该位置的值。CAS有效地说明了“我认为位置V应该包含值A；如果包含该值，则将B放到这个位置；否则，不要更改该位置，只告诉我这个位置现在的值即可。
>
> 一般来说在竞争不是特别激烈的时候，使用该包下的原子操作性能比使用 synchronized 关键字的方式高效的多(查看getAndSet()，可知如果资源竞争十分激烈的话，这个for循环可能换持续很久都不能成功跳出。不过这种情况可能需要考虑降低资源竞争才是)。

## CAS的底层实现

> 1 主要为三步，读取-比较-修改
>
> 比较是在检测是否有冲突，如果没有冲突，其他线程也可以修改这个值，所以需要保证  比较-修改的原子性
>
> 2  CAS底层是使用cpu指令集 cmpxchg保证其原子性。这个是x86和Intel架构中的compare and exchange指令。在多核情况下，这个指令也不能保证原子性，需要在指令前加上lock。lock指令可以保证一个cpu核心在操作期间独占一片内存区域。
>
> 3  在处理器中，一般有两种方式来实现上述效果，总线锁和缓存锁。在多核处理器的结构中，CPU核心并不能直接访问内存，而是统一通过一条总线访问。总线锁就是锁住这条总线，使其他核心无法访问内存。总线锁的内存代价太大，会导致其他核心停止工作。而缓存锁只是锁定部分内存区域。当一个cpu核心讲内存区域的数据读取到自己的缓存后，他会锁定缓存对应的内存区域。锁定期间，其他核心无法操作这块内存区域。
>
> CAS保证操作的原子性，但是并不保证变量的可见性，因此需要在变量上加上volatile关键字。

# AQS (AbstractQueuedSynchronizer)

>  AQS是并发容器J.U.C（java.lang.concurrent）下locks包内的一个类。它实现了一个**FIFO**(FirstIn、FisrtOut先进先出)的队列。底层实现的数据结构是一个**双向链表**

## 重要成员变量

>  volatile int state; //当前锁的状态。
>
>  所有对state的操作都是使用CAS。state=0表示没有任何线程持有这个锁，线程持有该锁后将state+1，释放时-1。多次持有释放则多次加减。
>
>  一个FIFO双向链表，双向链表中每一个节点都记录了线程信息，代表一个等待线程。
>
>  1. 请求锁
>
>    ```java
>    final void lock() {
>        if (compareAndSetState(0, 1))   
>    //没有线程持有锁时，直接获取锁，对应情况1
>            setExclusiveOwnerThread(Thread.currentThread()); 
>        else
>            acquire(1);
>    }
>    
>    public final void acquire(int arg) {
>        if (!tryAcquire(arg) && //如果当前线程已经持有锁，则使用 CAS 将 state 值加1，表示自己再次申请了锁，释放锁时减1。这就是可重入性的实现
>            acquireQueued(addWaiter(Node.EXCLUSIVE), arg)) //如果由其他线程持有锁，那么将自己添加进等待队列
>            selfInterrupt();
>    }
>    ```
>
>  2. 建Node节点并加入链表
>
>    ​	如果没有竞争到锁，上面的addWaiter方法中，会创建一个Node节点，并添加到链表的末尾，Node节点中持有当前线程的引用。Node节点中的另一个成员也需要我们注意：
>
>  ```java
>  volatile int waitStatus;  
>  // initial 值为0、表示当前没有线程获取锁（初始状态）
>  // SIGNAL 值为-1、后继节点的线程处于等待的状态、当前节点的线程如果释放了同步状态或者被取消、会通知后继节点、后继节点会获取锁并执行（当一个节点的状态为SIGNAL时就意味着在等待获取同步状态，前节点是头节点也就是获取同步状态的节点）
>  
>  // CANCELLED 值为1、因为超时或者中断，结点会被设置为取消状态，被取消状态的结点不应该去竞争锁，只能保持取消状态不变，不能转换为其他状态。处于这种状态的结点会被踢出队列，被GC回收（一旦节点状态值为1说明被取消，那么这个节点会从同步队列中删除）
>  
>  // CONDITION 值为-2、节点在等待队列中、节点线程等待在Condition、当其它线程对Condition调用了singal()方法该节点会从等待队列中移到同步队列中
>  
>  // PROPAGATE 值为-3、表示下一次共享式同步状态获取将会被无条件的被传播下去（读写锁中存在的状态，代表后续还有资源，可以多个线程同时拥有同步状态）
>  ```
>
>  另外，加到链表末尾的操作使用了CAS+死循环的模式。
>
>    1. 挂起等待
>
>       ​		如果此节点的上一个节点是头部节点，则再次尝试获取锁，获取到了就移除并返回。获取不到就进入下一步；
>
>       ​		判断前一个节点的 waitStatus，如果是 SINGAL，则返回 true，并调用 LockSupport.park() 将线程挂起；
>
>       如果是 CANCELLED，则将前一个节点移除；
>
>       如果是其他值，则将前一个节点的 waitStatus 标记为 SINGAL，进入下一次循环。
>
>    2. 释放锁
>
>       ​		调用 tryRelease，此方法由子类实现。实现非常简单，如果当前线程是持有锁的线程，就将 state 减1。减完后如果 state 大于0，表示当前线程仍然持有锁，返回 false。如果等于0，表示已经没有线程持有锁，返回 true，进入下一步；如果头部节点的 waitStatus 不等于0，则调用LockSupport.unpark()唤醒其下一个节点。头部节点的下一个节点就是等待队列中的第一个线程，这反映了 AQS 先进先出的特点。另外，即使是非公平锁，进入队列之后，还是得按顺序来。
>
>  ### 获取同步状态
>
>  ​	线程A要获取同步状态（这里想象成锁，方便理解），初始状态下state=0, A获取锁后将state置为1。在A没有释放锁期间，线程B也来获取锁，此时因为state=1，表示锁被占用，所以将B的线程信息和等待状态等信息构成出一个Node节点对象，放入同步队列，head和tail分别指向队列的头部和尾部（此时队列中有一个空的Node节点作为头点，head指向这个空节点，空Node的后继节点是B对应的Node节点，tail指向它），同时阻塞线程B(这里的阻塞使用的是LockSupport.park()方法)。后续如果再有线程要获取锁，都会加入队列尾部并阻塞。
>
>  ### 释放同步状态
>
>  当线程A释放锁时，即将state置为0，此时A会唤醒头节点的后继节点（所谓唤醒，其实是调用LockSupport.unpark(B)方法），即B线程从LockSupport.park()方法返回，此时B发现state已经为0，所以B线程可以顺利获取锁，B获取锁后B的Node节点随之出队。

# 读写锁实现机制

> 特性：
>
> 只有一个线程可以获取到写锁。
>
> 可以有多个线程获取到读锁。
>
> 实现原理：
>
> 读写锁虽然是两个锁，但是只有一个等待队列。
>
> 获取写锁时，要保证没有任何线程持有锁；
>
> 写锁释放后，会唤醒队列中的第一个线程，可能是读锁或写锁；
>
> 如果是读锁，先判断写锁有没有被持有，没有就可以获取成功；
>
> 读锁获取成功后，会继续按顺序唤醒队列中等待读锁的线程，直到遇到写锁线程；
>
> 释放读锁时，要检查读锁数，如果为0，则唤醒队列中下一个线程，否则不操作。