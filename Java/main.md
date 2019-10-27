# 一 10个经典的 Java main 方法面试题
## 1. 不用main方法如何定义一个类？
	不行，没有main方法我们不能运行Java类。
    在Java 7之前，你可以通过使用静态初始化运行Java类。但是，从Java 7开始就行不通了。
## 2. main()方法需要的参数不是字符串数组？
	不是的，main()方法的参数必须是字符串数组。但是，在引进变参时，你可以将字符串类型的变参作为参数传递给main()方法。变参一定得是数组。
	package com.instanceofjava;
	public class MainMethod{
	public static void main(String args[]){
	}
	}
## 3. 我们能不能改变main()方法的返回类型？
	不能，main()方法的返回类型只能是空。任何其它类型都是不能接受的。
  	package com.instanceofjava;
  	public class A{
      public static int main(String[] args){
          return 1;    //run time error : No main method found
  	}
 	 }
## 4. main()方法为什么必须是静态的？
    main()方法一定是静态的。如果main()允许是非静态的，那么在调用main方法时，JVM就得实例化它的类。在实例化时，还得调用类的构造函数。如果这个类的构造函数有参数，那么届时就会出现歧义。例如，在下面的程序中，在实例化类“A”的时候，JVM传递什么参数？
    package com.instanceofjava;
    public class A{
    public MainMethod(int i){
        //Constructor taking one argument
    }
    public void main(String[] args){
        //main method as non-static
    }
## 5. 我们能不能声明main()方法为非静态？
    不能，main()方法必须声明为静态的，这样JVM才可以调用main()方法而无需实例化它的类。
    如果从main()方法去掉“static”这个声明，虽然编译依然可以成功，但在运行时会导致程序失败。
    package com.instanceofjava;
    public class A{
    public void main(String[] args){
        System.out.println("indhu");         //Run time error
    }
    }
## 6. 我们能否重载main()方法？
    可以，我们可以重载main()方法。一个Java类可以有任意数量的main()方法。为了运行java类，类的main()方法应该有例如“public static void main(String[] args)”的声明。如果你对此声明做任何修改，编译也是可以成功的。但是，运行不了Java程序。你会得到运行时错误，因为找不到main方法。
    package com.instanceofjava;
    public class A{
    public static void main(String[] args){
        System.out.println("Indhu");
    }
    void main(int args){
        System.out.println("Sindhu");
    }
    long main(int i, long d){
        System.out.println("Saidesh");
    return d;
    }
    }
## 7.我们能否声明main()方法为private或protected，或者不用访问修饰符？
    不能，main()方法必须public。你不能定义main()方法为private和protected，也不能不用访问修饰符。这是为了能让JVM访问main()方法。如果你不定义main()方法为public，虽然编译也会成功，但你会得到运行时错误，因为找不到main方法。
    package com.instanceofjava;
    public class A{
    private static void main(String[] args){
        //Run time error
    }
    }
## 8.我们能否在Java中覆盖main方法？
    不能，你不能在Java中覆盖main方法。这是因为main方法是静态方法，而在Java中静态方法在编译时会结合在一起，所以你在Java中不能覆盖静态方法。
## 9.我们能否在Java中终结main方法？
    你可以在Java中终结main方法。JVM对此没问题。
## 10.我们能否在Java中同步main方法？
    是的，main方法可以在Java中同步，synchronized修饰符允许用于main方法的声明中，这样就可以在Java中同步main方法了