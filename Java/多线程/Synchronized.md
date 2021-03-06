- 修饰实例方法，作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁

 ```java
 public synchronized void increase() {
     i++;
 }
 ```

- 修饰静态方法，作用于当前类对象加锁，进入同步代码前要获得当前类对象的锁 。也就是给当前类加锁，会作用于类的所有对象实例，因为静态成员不属于任何一个实例对象，是类成员（ static 表明这是该类的一个静态资源，不管new了多少个对象，只有一份，所以对该类的所有对象都加了锁）。所以如果一个线程A调用一个实例对象的非静态 synchronized 方法，而线程B需要调用这个实例对象所属类的静态 synchronized 方法，是允许的，不会发生互斥现象，因为访问静态 synchronized 方法占用的锁是当前类的锁，而访问非静态synchronized 方法占用的锁是当前实例对象锁。

 ```java
 public static synchronized void increase() {
     i++;
 }
 ```

 

- 修饰代码块，指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。 和 synchronized 方法一样，synchronized(this)代码块也是锁定当前对象的。synchronized 关键字加到 static 静态方法和synchronized(class)代码块上都是是给 Class 类上锁。这里再提一下：synchronized关键字加到非 static 静态方法上是给对象实例上锁

 ```java
 //成员锁   锁的对象是变量
 public Object synMethod(Object a1) {
     synchronized(a1) {
         // 操作
     }
 }
 
 //实例对象锁   this 代表当前实例
 synchronized(this) {
     for (int j = 0; j < 100; j++) {
         i++;
     }
 }
 
 //当前类的 class 对象锁
 synchronized(AccountingSync.class) {
     for (int j = 0; j < 100; j++) {
         i++;
     }
 }
 
 ```

​    