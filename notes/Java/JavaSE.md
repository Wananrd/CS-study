* [JavaSE](#java基础)
  * [零、基础知识](#零基础知识)
    * [环境搭建](#环境搭建)
    * [命名规范](#命名规范)
    * [数据类型](#数据类型)
    * [输入输出](#输入输出)
    * [循环](#循环)
    * [数组](#数组)
  * [一、类](#一类)
    * [属性与方法](#属性与方法)
    * [包装类](#包装类)
    * [缓存池](#缓存池)
  * [二、关键字](#二-关键字)
    * [static](#static)
    * [final](#final)
  * [三、Object类](#三object类)
    * [equasl方法](#equals方法)
    * [toString方法](#tostring方法)
  * [四、继承](#四继承)
    * [访问权限](#访问权限)
    * [super](#super)
    * [重写与重载](#重写与重载)
    * [多态](#多态)
    * [抽象类与接口](#抽象类与接口)
  * [五、 接口与内部类](#五接口与内部类)
    * [简介](#简介)
    * [Comparator与Comparable](#comparator与comparable)
    * [内部类](#内部类)
      * [局部内部类](#局部内部类)
      * [匿名内部分(非常重要)](#匿名内部分非常重要)
      * [成员内部类](#成员内部类)
  * [六、异常](#六-异常)
  * [七、字符串](#七字符串)
    * [字符串用法](#字符串用法)
    * [String的不可变性](#string的不可变性)
    * [String，StringBuffer and StringBuilder](#stringstringbuffer-and-stringbuilder)
    * [String Pool](#string-pool)
    * [new String("abc")](#new-stringabc)
    * [类型转换](#类型转换)
  * [八、枚举类&注解](#八枚举类注解)
    * [自定义枚举类](#自定义枚举类)
    * [enum关键字](#enum关键字)
    * [注解(Annotation)](#注解annotation)
  * [九、泛型](#九泛型)
    * [作用](#作用)
    * [自定义泛型](#自定义泛型)
    * [通配符?](#通配符)
  * [十、反射](#十反射)
    * [Class类的实例获取](#class类的实例获取)
    * [创建运行时类的对象](#创建运行时类的对象)
    * [调用运行时类的指定结构](#调用运行时类的指定结构)
    * [动态代理](#动态代理)
  * [十一、其它类](#十一其它类)
    * [日期类](#日期类)


# Java基础

## 零、基础知识

### 环境搭建

JDK：编写Java程序的程序员使用的软件

JRE：运行Java程序的用户使用的软件

**安装顺序**

下载JDK，并为之设置环境变量 ==>>JRE

> 下载完成之后在cmd中运行javac --version查看版本

**使用命令行工具**

> javac Hello.java
>
> java Hello

javac是一个Java编译器，它将文件Hello.java编译成Hello.class。

java程序启动Java虚拟机，虚拟机执行编译器编译到类文件中的字节码。



### 命名规范

**类**：类名是以大写字母开头的名词。[FirstSample]

**源代码的文件名**：必须与公共类的名字相同[FirstSample.java]

**常量**：常量名使用全大写[public **static final** double CM_PER_INCH = 2.54;]





### 数据类型

**整型**：int[4字节]、short[2字节]、long[8字节]、byte[1字节]

> 从Java7开始，加上前缀0b或0B就代表二进制数，0x代表十六进制
>
> 还可以为数字字面量加下划线[1_000_000代表100万]

**浮点型**：float[4字节]、double[8字节]

> float类型的数值有一个后缀F或f。
>
> double类型后面加后缀D或d。[默认]

> 在进行金融计算的时候应该使用BigDecimal

**char**：编码最好采取UTF-8，比较通用

**boolean类型**：布尔类型不能等于整型[boolean flag = 0;]，只能等于true/false;



**大数**

在整数和浮点数精度不能够满足需求，那么可以使用`BigInteger`和`BigDecimal`这两个类。他们可以实现任意精度的运算。

运算的时候需要使用类中的相应方法，并将其中的类型使用`xxx.valueOf(a)`转换为响应的类型

~~~java
BigInteger d = c.mutiply(b.add(BigInteger.valueOf(2)));//d = c * 2;
~~~



### 输入输出

**读取输入**

要想通过控制台输入，首先需要创建一个与“标准输入流”System.in关联的Scanner对象。

~~~java
Scanner in = new Scanner(System.in);
System.out.print("What's your name?");
String name = in.nextLine();
System.out.println("My name is " + name);
System.out.print("How old are you?");
int age = in.nextInt();
System.out.print("My age is " + age);
~~~

* in.next()：读取输入的下一个单词
* in.nextLine()：读取下一行数据
* in.nextInt()：读取下一个表示整型的数据
* in.nextDouble()：读取下一个表示浮点数的数据



**格式化输出**

与C语言用法相同，语法为`System.out.printf(...)`



### 循环

循环跳出提供了一种`带标签的break语句`。执行带标签的break会跳转到带标签的语句末尾。

~~~java
Scanner in = new Scanner(System.in);
int n = 1;
read_data:
while(true){
    while(n != 0) {
        System.out.print("请输入n:");
        n = in.nextInt();
        if(n == 0) {
            break read_data;
        }
    }
}
System.out.print("跳出循环");
~~~



### 数组

**声明**

~~~java
int[] a;//或者int a[];
int[] a = new int[100];
~~~



**数组拷贝**

使用=号表面上拷贝过去了，实际上只是引用对象的地址相同。

~~~java
int smallPrimes[] = {1, 2, 3, 4, 5};
int[] luckyNumbers = smallPrimes;	
luckyNumbers[2] = 88;		
System.out.println(Arrays.toString(smallPrimes));//[1, 2, 88, 4, 5]
System.out.println(Arrays.toString(luckyNumbers));//[1, 2, 88, 4, 5]
~~~

应该使用Arrays类的copyOf方法

> 第一个参数为背拷贝的数组，第二个参数为需要拷贝的长度。

~~~java
int smallPrimes[] = {1, 2, 3, 4, 5};
int[] luckyNumbers = Arrays.copyOf(smallPrimes, smallPrimes.length);
luckyNumbers[2] = 88;
System.out.println(Arrays.toString(smallPrimes));//[1, 2, 3, 4, 5]
System.out.println(Arrays.toString(luckyNumbers));//[1, 2, 88, 4, 5]
~~~



**排序**

数组中的排序可以使用Arrays类中的sort方法。

数组中的排序采用了优化的快速排序。

~~~java
Arrays.sort(luckyNumbers);
~~~



**ArrayList**

虽说Java允许在运行时确定数组大小.

~~~java
int size = ...;
var staff = new Employee[size];
~~~

但并未能够完全解决运行时动态更改数组的问题。

Java中的解决方法就是使用`ArrayList`。



## 一、类



### 属性与方法

**属性别名**：成员变量、field、域、字段

**属性**与**局部变量**的异同：

**同**：

1. 格式相同：数据类型 变量名 = 变量值
2. 先声明后使用
3. 有其作用域

**异**：

1. 属性直接定义在{}内
2. 属性拥有权限修饰
3. **属性有初始值**
4. **属性加载到堆空间，局部变量加载到栈空间**



**方法**

程序设计中函数的传参有两种：**按值传递**和**引用传递**

Java语言总是采用**按值传递**。【Java中两个数的交换如何实现？】





### 包装类

**自动拆箱与自动装箱**

~~~java
Integer a = 2;	//自动装箱 调用了 Integer.valueof(2);
int b = a;		//自动拆箱 调用了 a.intValue();
~~~

**String与Integer(Int)类型转换**

~~~java
String str = String.valueof(12.2f);//float-->>String
float f = Float.parseFloat(str);//String-->>float
~~~

**注意**：

> 当使用parseFloat时可能会出现NumberFormatException异常

### 缓存池

new Integer(123)与Integer.valueOf(123)的区别：

* new Integer(123)会每次新建一个对象

* Integer.valueOf(123)会使用缓存池中的对象

~~~java
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true
~~~

  在JDK8中，Integer缓存池的大小默认为-128~127



## 二、 关键字



### static

1. **静态属性**

  * 静态变量：又称类变量，也就是说**这个变量属于类的**，类的所有实例都共享一份数据，可以直接通过类名来访问。静态变量在内存中只存在一份，存在方法区的静态域中。
  * 实例变量：每创建一个实例就会产生一个实例变量，它与该实例同生共死。

~~~java
public class A {

    private int x;         // 实例变量
    private static int y;  // 静态变量

    public static void main(String[] args) {
        // int x = A.x;  // Non-static field 'x' cannot be referenced from a static context
        A a = new A();
        int x = a.x;
        int y = A.y;
    }
}
~~~

2. **静态方法**

  * 随着类的加载而加载，不依赖任何对象，所以静态方法必须有实现，并且只能调用静态属性。

~~~java
public abstract class A {
    public static void func1(){
    }
    // public abstract static void func2();  // Illegal combination of modifiers: 'abstract' and 'static'
}
~~~

   **注意点**

  * 在静态方法内，不能使用this关键字，super关键字(生命周期)

3. **静态代码块**

   静态代码块在类初始化的时候运行一次。

~~~java
public class A {
    static {
        System.out.println("123");
    }

    public static void main(String[] args) {
        A a1 = new A();
        A a2 = new A();
    }
}
~~~

~~~java
123
~~~

4. **静态内部类**

5. **静态导包**

   在使用静态变量和方法时不用再指明类名，简化代码，降低可读性。

   ~~~java
   import static com.xxx.ClassName.*
   ~~~

6. **初始化顺序**

   由父及子，静态先行

7. **使用建议**(在下面两种情况下可以使用静态方法)

  * 工具类
  * 方法只需要访问静态字段



### final



可以用来修饰：类、方法、变量

1. **修饰类**

   此类不能被继承

2. **修饰方法**

   此类不能被重写

3. **修饰变量**

   修饰变量时，此时“变量”就是一个常量

4. **static final修饰属性**：全局变量



## 三、Object类

Object类是所有类的父类



### equals方法

1. **equals与==比较**

  * 对于基本类型，==判断两个值是否相等，基本类型没有equals()方法
  * 对于引用类型，==判断两个变量是否引用同一个对象，而equals()判断引用的对象是否等价

~~~java
Integer x = new Integer(1);
Integer y = new Integer(1);
System.out.println(x.equals(y)); // true
System.out.println(x == y);      // false
~~~

2. 开发中实现快捷键equ，之后可以手动重写

3. **equals方法重写**

   一般需要比较两个自定义对象的时候，都会选择重写equals。下面给出一种比较良好的equals方法。

~~~java
public class Employee
{
    ... 
    public boolean equals(Object obj)
    {
        //判断两个对象是否引用同一个对象
        if(this == obj) return true;
        
        //obj不能为null
        if(obj == null) return false;
        
        //两个对象不是同一个类
        if(getClass() != ojb.getClass()) return false;
        
        //属于同一个类，可以进行类型转换
        Employee object = (Employee) obj;
        
        //判断域是否相同...
        return name.equals(object.name) && salary.equals(object.salary);
    }
}
~~~


补充：如果重写了equals方法，就必须为用户重新定义hashCode方法。




### toString方法

1. Object类中toString()的定义

   ~~~java
    public String toString() {
    return
    getClass().getName() + "@" + Integer.toHexString(hashCode());
    }
   ~~~

2. 开发中快捷键：tos



## 四、继承



### 访问权限

java中共有三个访问权限修饰符：private、default(一般缺省)、protected、public

| 权限      | 类内 | 同包 | 不同包子类 | 不同包非子类 |
| --------- | ---- | ---- | ---------- | ------------ |
| private   | √    | ×    | ×          | ×            |
| default   | √    | √    | ×          | ×            |
| protected | √    | √    | √          | ×            |
| public    | √    | √    | √          | √            |



### super

* 访问父类的构造函数：可以用super()函数访问父类的构造函数，从而委托父类完成一些初始化工作。子类一定会调用父类的构造函数来完成初始化工作，一般调用父类的默认构造函数，如果子类需要调用父类其它构造函数，那么就可以使用super()函数
* 访问父类的成员：如果子类重写了父类的某个方法，可以通过super关键字来引用父类的方法实现。
* 同名属性同父类



### 重写与重载

1. **重写(Override)**

   子类继承父类以后，可以对父类中同名同参数的方法进行覆盖操作。

   三个限制：

  * 子类方法的访问权限必须大于父类方法
  * 子类方法的返回类型是父类方法返回类型或其子类
  * 子类方法抛出的异常类型必须是父类抛出的异常类型或其子类

2. **重载(Overload)**

   存在于同一个类中，指一个方法与已经存在的方法名称上相同，单数参数类型、个数、顺序至少有一个不同。

   **注意**：返回值不同，其它都相同不算重载。

   ~~~java
    class OverloadingExample {
    public void show(int x) {
    System.out.println(x);
    }
    
        public void show(int x, String y) {
            System.out.println(x + " " + y);
        }
    }
   ~~~

   ~~~java
    public static void main(String[] args) {
    OverloadingExample example = new OverloadingExample();
    example.show(1);
    example.show(1, "2");
    }
   ~~~



### 多态

1. **理解**：一个事物的多种形态

2. **何谓多态性**：父类的引用指向子类的对象（或子类的对象赋给父类的引用）

   ~~~java
    Person p = new Man();
    Object obj = new Date();
   ~~~

3. 多态性的**使用**：编译看左运行看右

   **前提**：类的**继承**关系与方法的**重写**

4. **注意点**：只适用于方法，不适用于属性

5. 关于向上转型与向下转型

   向上转型：多态

   向下转型：强制类型转换

  * 注意点：使用强制类型转换时，可能出现ClassCastException

  * 为了避免异常可以使用instanceof

    ~~~java
    a instanceof A:判断对象a是否是类A的实例。如果是，返回true；如果不是，返回false
    ~~~



### 抽象类与接口

1. **抽象类**

  * 修饰类

    > 此类不能实例化。
    >
    > 抽象类中一定有构造器，便于让子类实例化调用。
    >
    > 开发中，都会提供抽象类的子类，让子类实例化，完成相应的操作。
    >
    > 抽象类使用的**前提**：继承性。

  * 修饰方法

    > 抽象方法只有方法的声明，没有方法体。
    >
    > 包含抽象方法的类一定是抽象类。反之不成立。
    >
    > 若子类重写了父类所有的抽象方法后，此子类可实例化。
    >
    > 若子类未能重写父类所有的抽象方法，此子类也是抽象类。

  * **注意点**

    1. abstract不能用来修饰：属性、构造器等结构
    2. abstract不能用来修饰私有方法、静态方法、final方法、final的类。

2. **接口**

   接口是抽象类的延伸，在 Java 8 之前，它可以看成是一个完全抽象的类，也就是说它不能有任何的方法实现。

   从 Java 8 开始，接口也可以拥有默认的方法实现，这是因为不支持默认方法的接口的维护成本太高了。在 Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类，让它们都实现新增的方法。

   接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected。从 Java 9 开始，允许将方法定义为 private，这样就能定义某些复用的代码又不会把方法暴露出去。

   接口的字段默认都是 static 和 final 的。



## 五、接口与内部类

### 简介

**概念：**接口是对希望复合这个接口类的一组需求。

**接口中的方法**：接口中的所有方法都自动是是public方法，因此方法声明时，不必提供关键字public。

接口中不会有实例字段。在Java8之前，接口中绝对不会实现方法



**接口的属性**

接口不是类，接口不能实例化。

却能声明接口的变量。

可以使用`instanceof`检查一个对象是否实现了某个特定的接口。

虽然接口中不能包含实例字段，但是可以包含**常量**。



**静态和私有方法**

在`Java8`中，允许在接口中增加`静态方法`。通常做法都是将静态方法放在`伴随类`中。

`Java9`中，接口的方法可以是`private`。private方法可以是静态方法或实例方法，只能作为接口中其他方法的辅助方法。



**默认方法**

可以为接口方法实现一个默认方法，必须用`default`修饰这样一个方法

~~~java
public interface Comparable<T>
{
    default int compareTo(T other){return 0;}
}
~~~



**默认方法的好处**

假设一个类实现了一个接口，接口更改了(多了一个方法)，假如没有默认方法，那么该类将无法编译。不能保证`源代码兼容`。



**解决默认方法冲突**

加入现在一个接口中将一个方法定义为默认方法，然后又在超类或另一个接口中定义同样的方法，会发生什么？

Java给出的规则如下：

1. 超类优先

   > 如果超类提供了一个具体的方法，同名而且有相同参数的默认方法会被忽略。

2. 接口冲突

   > 如果一个接口提供了一个默认方法，另一个接口提供了一个同名而且参数类型相同的方法，必须覆盖这个方法来解决冲突。



### Comparator与Comparable

对对象数组进行排序，有两种方式。

即`Comparator`与`Comparable`

**方式一：Comparator**：对象实现Comparable接口

~~~java
public class Employee implements Comparable<Employee>{
	public static void main(String[] args) {
		Employee[] staff = new Employee[3];
		staff[0] = new Employee("Harry", 35000);
		staff[1] = new Employee("Carl", 75000);
		staff[2] = new Employee("Testera", 38000);
		
		Arrays.sort(staff);
		for(Employee e : staff) {
			System.out.println(e);
		}
	}
	private String name;
	private double salary;
	public Employee(String name, double salary) {
		super();
		this.name = name;
		this.salary = salary;
	}
	public int compareTo(Employee o) {
		return this.name.length() - o.name.length();
	}
	public String toString() {
		return "Employee [name=" + name + ", salary=" + salary + "]";
	}
}
~~~

**方式二：Comparable**：采用一个数组+比较器的形式

~~~java
public class EmployeeSort2{
	public static void main(String[] args) {
		String[] friends = {"Peter", "Amy", "Mary"};
		Arrays.sort(friends, new myComparator());
		
		for(int i = 0; i < 3; i ++) {
			System.out.println(friends[i]);
		}
		
	}
}
class myComparator implements Comparator<String>
{
	@Override
	public int compare(String o1, String o2) {
		return -(o1.length() - o2.length());
	}
}
~~~



### 内部类

一共有4种分类。

> 定义在外部类的局部位置上(比如方法内)

* 局部内部类(有类名)
* **匿名内部类(无类名，重点!!!!!!!)**

> 定义在外部类的成员位置上

* 成员内部类(没用static修饰)
* 静态内部类(使用static修饰)



#### 局部内部类

1. 可以直接访问外部类的所有成员，包括私有的。

   ~~~java
    class Outer01{//外部类
        private int n1 = 100;
        public void m1() {//方法体
            class Inner01{//局部内部类
                public void f1() {
                    System.out.println("n1 = " + n1);//访问私有
                }
            }
        }
    }
   ~~~

2. 不能添加访问修饰符，但是可以使用`final`修饰，被final修饰不能被继承。

   ~~~java
   final class Inner01{//局部内部类
   ~~~

3. 定义和作用域在`方法体`或者`代码块`中。

4. 如果外部类和局部内部类的成员重名时，默认遵循`就近原则`，如果想访问外部类的成员，则可以使用（`外部类名.this.成员`）去访问

   ~~~java
   System.out.println("n1 = " + n1 + "  外部类的n1 = " + Outer02.this.n1);
   ~~~

5. `外部其它类`不能直接访问局部内部类



完整代码：

~~~java
package com.javacore.clazz;

public class LocalInnerClass {
	public static void main(String[] args) {
		Outer02 outer = new Outer02();
		outer.m1();
		System.out.println("outer  hascode   " + outer);//验证		
	}
}

class Outer02{//外部类
	private int n1 = 100;
	private void m2() {System.out.println("外部类的m2()");	}
	public void m1() {//方法体
		final class Inner01{//局部内部类
			private int n1 = 800;
			public void f1() {
				//Outer02.this代表调用m1的对象，这里是outer,可以验证hashcode
				System.out.println("n1 = " + n1 + "  外部类的n1 = " + Outer02.this.n1);//800	100
				System.out.println("Outer02.this hascode" + Outer02.this);//验证
				m2();
			}
		}
		Inner01 inner01 = new Inner01();
		inner01.f1();
	}
}
~~~







#### 匿名内部分(非常重要)

1. 本质是类
2. 内部类
3. 该类没有名字[实则在底层会分配 `外部类$(n)`]
4. 同时还是一个对象



**基本语法**

~~~java
new 类或接口(参数列表){
    类体
}；
~~~



下面是一些基于接口、类、抽象类以及直接使用方法的匿名内部类的实现。

~~~java
package com.javacore.clazz;

public class AnonymousInnerClass {
	public static void main(String[] args) {
		Outer outerr = new Outer();
		outerr.method();  	
	}
}

class Outer{//外部类
	private int n1 = 10;//属性
	public void method() {//方法
		//基于接口的匿名内部类 ，类似于实现接口
		A tiger = new A() {
			public void cry() {
				System.out.println("老虎在叫......");
			}
		};
		System.out.println(tiger.getClass());//Outer$1【名字 ： 外部类+$+第几个匿名内部类】
		tiger.cry();
		
		//基于类的匿名内部类，类似于继承
		Father father = new Father("WananRd") {//传递参数，调用父类构造函数
			public void eat() {
				System.out.println("人吃饭");
			}
		};
		System.out.println(father.getClass());//Outer$2
		father.eat();
		
		//基于抽象类的匿名内部类,抽象类必须有实现
		Animal animal = new Animal() {
			void fly() {
				System.out.println("动物飞");
			}
		};
		animal.fly();

		//可以不用创建对象，直接实现其方法
		new Animal() {
			void fly() {
				System.out.println("蝙蝠侠会飞");
			}
		}.fly();
	}
}

interface A{
	public void cry();
}

class Father{
	private int n1 = 99;
	public Father(String name) {
		System.out.println("name = " + name);
	}
	public void eat() {
		System.out.println("父亲吃饭");
	}
}
abstract class Animal{
	abstract void fly();
}
~~~





**匿名内部类的最佳实践**

* 当做实参直接传递，简洁高效。

~~~java
public class Example {

	public static void main(String[] args) {
		f1(new AA(){
			public void show() {
				System.out.println("这是一幅名画。。。");
			}
		});
	}
	
	//静态方法，形参是接口类型
	public static void f1(AA aa) {
		aa.show();
	}
}

//接口
interface AA{
	void show();
}
~~~



#### 成员内部类

1. 定义在`外部类的成员`位置上

2. 可以添加任意访问修饰符(`public、protected、默认、private`)

3. 外部类使用成员内部类的两种方式

   ~~~java
    //方式一：
    //相当于new Inner01()当做outer01的成员
    Outer01.Inner01 inner01 = outer01.new Inner01();
    inner01.say();
    
    //方式二：在外部类中编写一个方法
    Outer01.Inner01 inner02 = outer01.getInner01Instance();
    inner02.say();
   ~~~



完整代码：

~~~java
package com.javacore.clazz;

public class InnerClass {

	public static void main(String[] args) {
		Outer01 outer01 = new Outer01();
		outer01.t1();
		
		//外部其它类，使用成员内部类的两种方式
		//方式一：
		//相当于new Inner01()当做outer01的成员
		Outer01.Inner01 inner01 = outer01.new Inner01();
		inner01.say();
		
		//方式二：在外部类中编写一个方法
		Outer01.Inner01 inner02 = outer01.getInner01Instance();
		inner02.say();
	}
}

class Outer01{
	private int n1 = 10;
	private String name = "张三";
	
	//1. 成员内部类，是定义在外部内的成员位置上
	//2. 可以添加任意访问修饰符(public、protected、默认、private)
	public class Inner01{//成员内部类
		public void say() {
			//可以直接访问外部类的所有成员，包含私有的
			System.out.println("n1 = " + n1 + " name = " + name);
		}
	}
	
	//写方法
	public void t1() {
		//使用成员内部类
		Inner01 inner01 = new Inner01();
		inner01.say();
	}
	
	public Inner01 getInner01Instance() {
		return new Inner01();
	}
}
~~~



**静态内部类**：与成员内部类大体相同，唯一区别是前面增加了关键字`static`，并且不能够访问非静态成员。



## 六. 异常

1. **异常的分类**

   ~~~java
    * 一、异常的体系结构
    * java.lang.Throwable
    *      |----java.lang.Error:一般不编写针对性的对吗进行处理
    *      |----java.lang.Exception:可以进行异常处理
    *          |----编译时异常(checked)
    *              |----IOException
    *                  |----FileNotFoundException
    *              |----ClassNotFoundException
    *          |----运行时异常(unchecked, RuntimeException)
    *              |----NullPointerException
    *              |----ArrayIndexOutOfBoundsException
    *              |----ClassCastException
    *              |----NumberFormatException
    *              |----InputMismatchException
    *              |----ArithmeticException    
   ~~~

   **Exception**中分为两种异常：

   **checked**(受检异常)：需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复。

   **unchecked**(非受检异常)：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复。

2. **异常的处理**

  * **try-catch-finally**

    ~~~java
    try{
        //可能出现异常的代码
    }catch(异常类型1 变量名1){
        //处理异常的方式1
    }catch(异常类型2 变量名2){
        //处理异常的方式2
    }
    ...
    finally{
        //一定会执行的代码
    }
    ~~~

    说明：

    1. finally是可选的。

    2. 使用try将可能出现异常代码包装起来，在执行过程中，一旦出现异常，就会生成一个对应异常类的对象，根据此对象的类型，去catch中进行匹配

    3. 一旦try中的异常对象匹配到某一个catch时,就进入catch中进行异常处理。一旦处理完成，就跳出当前的try-catch结构（在没有finally情况下）。继续执行其后的代码。

    4. catch中的异常类型如果没有子父类关系，则谁声明在上，谁声明在下无所谓。

       catch中的异常类型如果满足子父类关系，则要求子类一定声明在父类的上面。否则，报错.

    5. 常用的异常处理的方式：①String getMessage() ②printStackTrace()。

    6. 在try结构中声明的变量，再出了try结构以后，就不能再被调用

    7. try-catch-finally结构可以嵌套

  * **throws**

    ~~~java
    “throws + 异常类型”写在方法的声明处。指明此方法执行时，可能会抛出的异常类型
    *     一旦当方法执行时，出现异常，仍会在异常代码出生成一个异常类的对象满足throws后
    *     异常类型时，就会抛出。异常代码后续的代码，就不再执行
    ~~~

  * 对比两种处理方式

    **try-catch-finally**：真正的将异常给处理掉了。

    **throws**的方式只是将异常抛给了方法的调用者。并没有真正的将异常处理掉。

3. **手动抛出异常对象**

  * **throw**

4. **自定义异常类**

   **如何自定义异常类**

  1. 继承于现有的异常结构：RuntimeException、Exception
  2. 提供全局常量serialVersionUID
  3. 提供重载构造器

~~~java
public class MyException extends Exception{
  static final long serialVersionUID = -7034897190745766939L;


  public MyException(String msg){
    super(msg);
  }
}

class Student{
  private int id;

  public void regist(int id) throws MyException {
    if(id > 0){
      this.id = id;
    }
    else{
      throw new MyException("不能输入负数");
    }
  }
}
~~~


5. **异常的使用技巧**

   异常处理不能代替简单的测试。

   > 捕获异常的时间非常之久，因而可以不适用异常，尽量不适用

   充分利用异常的层次结构

   > 如果能将一种异常转换为另一种更为合适的异常，那么不要犹豫。

   早抛出，晚捕获

   > 大量的try-catch代码影像可读性，因此可以抛出异常就抛出异常，尽量在上层捕获异常。

## 七、字符串

String声明为final，不可被继承的。

在Java8中，String内部使用char数组进行存储数据。

~~~java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
~~~

在Java9之后，String类的实现改用byte数组存储字符串，同时使用coder来标识使用了哪种编码。

~~~java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
~~~

value数组被声明为final，这意味着value数组初始化后就不能再引用其它数组。并且String内部没有改变value数组的方法，因此可以保证不可变。

### 字符串用法

**子串**

~~~java
String greeting = "Hello";
String s = greeting.substring(0, 3);//s = "Hel";
~~~

**拼接**

任何一个字符串与一个对象进行拼接时，后者都会转换为字符串。

~~~java
int age = 13;
String rating = "PG" + age;
~~~

**不可变性**

String中的内容不能够修改，如果要修改的话，可以使用拼接。

~~~java
String greeting = "Hello!";
greeting = greeting.substring(0, 3) + "p!";//greeting = "Help!";
~~~

**检测字符串是否相等**

~~~java
String p = "o!";
String s = "Hello!";
String t = "Hell" + p;
System.out.println(s == t);//false
System.out.println(s.equals(t));//true
~~~

一定不要使用==运算符检测两个字符串是否相等！这个运算符只能确定两个字符串是否存放在同一个位置上。

**检查字符串是否为空**

~~~java
if(str != null && str.length() != 0)
~~~

**码点**

~~~java
String str = "Hello";
char first = str.charAt(0);//'H'
~~~





### String的不可变性

**好处**

1. **可以缓存hash值**

   因为String的hash值经常被使用。

   例如：String做HashMap的key。不可变的特性可以使得hash值也不可变，因此只需要进行一次计算。

2. **String Pool的需要**

   如果一个String对象已经被创建过了，那么就会从String Pool中取得引用。只有String是不可变的，才可能使用String Pool。

3. **安全性**

   String经常作为参数，String不可变性可以保证参数不可变。

   例如：在作为网络连接参数的情况下如果String是可变的，那么在网络连接过程中，String被改变，改变String的那一方以为现在连接的是其它主机，而实际情况却不一定是。

4. **线程安全**

   String不可变性具备线程安全，可以在多个线程中安全地使用。

### String，StringBuffer and StringBuilder

1. **可变性**

  * String不可变
  * StringBuffer和StringBuilder可变

2. **线程安全**

  * String不可变，因此是线程安全的
  * StringBuilder不是线程安全的，效率高
  * StringBuffer是线程安全的，内部使用synchronized进行同步，效率低

3. **内存解析**

    ~~~java
    StringBuffer sb = new StringBuffer();//new char[16];底层创建了一个长度是16的数组
    StringBuffer sb2 = new StringBuffer("abc");//char[] value = new char["abc".length() + 16];
    ~~~

   扩容问题：如果要添加的数据底层char数组装不下，那就需要进行扩容。

  * 默认情况下，扩容为原来容量的2倍+2，同时将原有的数组重点元素复制到新的数组中。
  * 指导意义，开发中建议使用StringBuffer(int capacity)或StringBuilder(int capacity)。

### String Pool

字符串常量池保存着字符串字面量，这些字面量在编译时期就确定。还可以使用String的intern()方法在与逆行过程将字符串添加到String Pool中。

当调用intern()方法时，如果String Pool中已经存在一个字符串和该字符串值相等(equal()方法确定)，那么就会返回String Pool中字符串的引用；否则就会在String Pool中添加一个新的字符串，并返回这个字符串的引用。

代码示例：

其中s1和s2采用new的方式创建了两个字符串。s1和s2会在堆中创建两个地址，两个地址指向同一个常量池中对象。

s3和s4采用intern()方法，返回的是同一个常量池中的地址。

s5和s6这种字面量的形式创建的字符串，会自动地将字符串放入常量池中。

~~~java
String s1 = new String("aaa");
String s2 = new String("aaa");
System.out.println(s1 == s2);           // false

String s3 = s1.intern();
String s4 = s2.intern();
System.out.println(s3 == s4);           // true

String s5 = "bbb";
String s6 = "bbb";
System.out.println(s5 == s6);  // true
~~~

在Java6时，String Pool在方法区(永久区)

在Java7时，String Pool在堆空间，因为永久区的空间有限

在Java8时，String Pool在方法区(元空间)



### new String("abc")

这种方式会创建两个对象

* "abc"属于字面量，因此会在编译期会在String Pool中创建一个字符串对象。
* 使用new的方式会在堆中创建一个对象，该对象指向String Pool中的“abc”。



### 类型转换

1. **与基本数据类型、包装类的转换**

   String	--->>	基本数据类型、包装类：调用包装类的静态方法:parseXxx(str)

   ~~~java
   String s1 = "123";
   int num = Integer.parseInt(s1);
   ~~~

   基本数据类型、包装类	--->>	String：调用String重载的valueOf(xxx)

   ~~~java
   int num = 123;
   String s2 = String.valueOf(num);
   ~~~

2. **与字符数组之间的转换**

   String	--->>	char[]：调用String的toCharArray()

   ~~~java
   String s1 = "abc123";
   char[] charArray = s1.toCharArray();
   ~~~

   char[]	--->>	String：调用String构造器

   ~~~java
   char[] arr = new char[]{'h', 'e', 'l', 'l', 'o'};
   String str = new String(arr);
   ~~~

3. **与字节数组之间的转换**

   String	--->>	byte[]：调用String的getBytes()

   > 又称为编码

   ~~~java
   String s1 = "abc123中国";
   byte[] bytes1 = s1.getBytes();//使用默认的字符集UTF-8，进行转换
  
   byte[] bytes2 = s1.getBytes("gbk");//指定字符集标准gbk
   ~~~

   byte[]	--->>	String

   > 又称为解码

   ~~~java
   String s3 = new String(bytes1);//使用默认的字符集UTF-8，进行解码
  
   String s4 = new String(bytes2);//编码(gbk)与解码(UTF-8)不同,出现乱码
  
   String s5 = new String(bytes2, "gbk");//指定解码方式(gbk)
   ~~~



## 八、枚举类&注解

当需要定义一组常量时，强烈建议使用枚举类。



### 自定义枚举类

```java
class Season{
    //1. 声明Season对象的属性
    private final String seasonName;
    private final String seasonDesc;

    //2. 私有化类的构造器，并给对象属性赋值
    private Season(String seasonName, String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    //3. 提供当前枚举类的多个对象:public static final的
    public static final Season SPRING = new Season("春天", "春暖花开");
    public static final Season SUMMER = new Season("夏天", "夏日炎炎");
    public static final Season AUTUMN = new Season("秋天", "秋高气爽");
    public static final Season WINTER = new Season("冬天", "冰天雪地");

    //4. 其他诉求1：获取枚举类对象的属性
    public String getSeasonDesc() { return seasonDesc; }
    public String getSeasonName() { return seasonName; }
}
```



### enum关键字

```java
enum Season1 implements Info{
    //1. 提供当前枚举类的对象，多个对象之间用“,”隔开，末尾对象“;"结束
    SPRING("春天", "春暖花开"),
    SUMMER("夏天", "夏日炎炎"),
    AUTUMN("秋天", "秋高气爽"),
    WINTER("冬天", "冰天雪地");

    //2. 声明Season对象的属性
    private final String seasonName;
    private final String seasonDesc;

    //3. 私有化类的构造器，并给对象属性赋值
    private Season1(String seasonName, String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    //4. 其他诉求：获取枚举类对象的属性
    public String getSeasonDesc() { return seasonDesc; }
    public String getSeasonName() { return seasonName; }

    @Override
    public void show() {
        System.out.println("现在是" + getSeasonName());
    }
}
```



### 注解(Annotation)

注解是jdk5新增的功能。Annotation 其实就是代码里的特殊标记, 这些标记可以在编译, 类加载, 运行时被读取, 并执行相应的处理。通过使用 Annotation, 程序员可以在不改变原有逻辑的情况下, 在源文件中嵌入一些补充信息。

**框架 = 注解 + 反射 + 设计模式**



**注解的作用**

1. 生成文档相关的注解

2. 在编译时进行格式检查(JDK内置的三个基本注解)

   @Override: 限定重写父类方法, 该注解只能用于方法

   @Deprecated: 用于表示所修饰的元素(类, 方法等)已过时。通常是因为所修饰的结构危险或存在更好的选择

   @SuppressWarnings: 抑制编译器警告

3. 跟踪代码依赖性，实现替代配置文件功能



**如何自定义注解**：参考SuppressWarning定义

1. 注解声明为：@interface
2. 内部定义成员，通常使用value表示
3. 可以指定成员的默认值，使用default定义
4. 如果自定义注解没有成员，表明是一个标识作用

代码举例：

~~~java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
public @interface MyAnnotations {
    MyAnnotaion[] value();
}
~~~



**元注解**：对现有的注解进行解释说明的注解。

jdk提供了四个元注解：

* Retention：指定为Annotation的声明周期：SOURCE\CLASS(默认行为)\RUNTIME

  ​					  只有声明为RUNTIME声明周期的注解，才能通过反射获取。

* Target：用于指定被修饰的Annotation能用于修饰哪些程序元素

* Documented：用于指定被该元 Annotation 修饰的 Annotation 类将被javadoc 工具提取成文档。默认情况下，javadoc是不包括注解的。

* Inherited：被它修饰的 Annotation 将具有继承性。



JDK8中注解的新特性：可重复注解、类型注解

* 可重复注解：

  在MyAnnotation上声明@Repeatable，成员值为MyAnnotations.class

  MyAnnotation的Target和Retention和MyAnnotations相同

* 类型注解：

  ElementType.TYPE_PARAMETER 表示该注解能写在类型变量的声明语句中(如:泛型声明)。

  ElementType.TYPE_USE 表示该注解能写在使用类型的任何语句中。



## 九、泛型

### 作用

在对集合进行操作的时候，在集合中添加不同类型的元素能够添加成功，编译也不会报错，在使用强制类型转换之时便会报java.lang.ClassCastException的异常。

~~~java
ArrayList list = new ArrayList();
//需求：存放学生的成绩
list.add(78);
list.add(74);
//问题一：类型不安全
list.add("Tom");
    
for(Object score : list){
    //问题二：强转时，可能出现ClassCastException
    int stuScore = (Integer) score;
    System.out.println(stuScore);
}
~~~

于是引入了泛型，泛型可以在编译时就进行类型检查

~~~java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(73);
list.add(88);
list.add(99);
//编译时，就会进行类型检查

list.add("Tom");//编译不通过

for(Integer score : list){
    int stuScore = score;
    System.out.println(stuScore);
}
~~~



### 自定义泛型

**泛型类**

~~~java
public class Order<T> {
    String orderName;
    int orderId;
    T orderT;

    public Order(String orderName, int orderId, T orderT){
        this.orderName = orderName;
        this.orderId = orderId;
        this.orderT = orderT;
    }

    public void setOrderT(T orderT) {
        this.orderT = orderT;
    }
}
~~~

**泛型接口**

~~~java
public interface Generator<T> {
    public T next();
}
~~~

**泛型方法**

~~~java
public List<T> getForList(int index){
    return null;
}
~~~



### 通配符?

~~~java
Object obj = null;
String str = null;
obj = str;//编译通过

List<Object> list1 = null;
List<String> list2 = null;
list1 = list2;//编译不通过
~~~

使用?可以通过编译

~~~java
List<Object> list1 = new ArrayList<>();
List<String> list2 = null;

List<?> list = null;
list = list1;
System.out.println(list);
list = list2;
System.out.println(list);

//对于list<?>不能向其内部添加数据，null除外
//list.add("aa");
~~~

**限制条件**

`<? extends A>`：<= A

`<? super A>`：>= A

举例说明：
~~~java
//创建泛型类
class Generic<T>{
  private T key;

  public Generic(T key) {
    this.key = key;
  }

  public T getKey(){
    return key;
  }
}
~~~

~~~java
//使用方法验证
public void showKeyValue1(Generic<? extends Number> obj){
    System.out.println("key value is " + obj.getKey());
}
~~~


```java
Generic<String> generic1 = new Generic<>("String");
Generic<Integer> generic2 = new Generic<>(2222);
Generic<Float> generic3 = new Generic<>(222F);
Generic<Double> generic4 = new Generic<>(4.444);

//编译错误，因为String类型不是Number类型的子类
//        showKeyValue1(generic1);
showKeyValue1(generic2);
showKeyValue1(generic3);
showKeyValue1(generic4);
```




## 十、反射

Reflection（反射）是被视为动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。

Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库主要包含了以下三个类：

- **Field** ：可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段；
- **Method** ：可以使用 invoke() 方法调用与 Method 对象关联的方法；
- **Constructor** ：可以用 Constructor 的 newInstance() 创建新的对象。

**反射的优点**： 

* **可扩展性** ：应用程序可以利用全限定名创建可扩展对象的实例，来使用来自外部的用户自定义类。
* **类浏览器和可视化开发环境** ：一个类浏览器需要可以枚举类的成员。可视化开发环境（如 IDE）可以从利用反射中可用的类型信息中受益，以帮助程序员编写正确的代码。
* **调试器和测试工具** ： 调试器需要能够检查一个类里的私有成员。测试工具可以利用反射来自动地调用类里定义的可被发现的 API 定义，以确保一组测试中有较高的代码覆盖率。

**反射的缺点**：

* **性能开销** ：反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。因此，反射操作的效率要比那些非反射操作低得多。我们应该避免在经常被执行的代码或对性能要求很高的程序中使用反射。
* **安全限制** ：使用反射技术要求程序必须在一个没有安全限制的环境中运行。如果一个程序必须在有安全限制的环境中运行，如 Applet，那么这就是个问题了。
* **内部暴露** ：由于反射允许代码执行一些在正常情况下不被允许的操作（比如访问私有的属性和方法），所以使用反射可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性。反射代码破坏了抽象性，因此当平台发生改变的时候，代码的行为就有可能也随着变化。



### Class类的实例获取

获取Class类的几种方式

* 方式一：调用运行时类的属性.class

  ~~~java
  Class clazz1 = Person.class;
  ~~~

* 方式二：通过运行时类的对象

  ~~~java
  Person p1 = new Person();Class clazz2 = p1.getClass();
  ~~~

* **方式三：调用Class的静态方法：forName(String classPath)**

  ~~~java
  Class clazz3 = Class.forName("reflect.Person");//能够体现动态性
  ~~~



### 创建运行时类的对象

~~~java
Class<Person> clazz = Person.class;
Person obj = clazz.newInstance();
System.out.println(obj);
~~~

说明：调用此方法，创建对应的运行时类的对象.内部调用了运行时类的空参构造器。

要想此方法的正常创建运行时类的对象，

要求：运行时类必须提供空参的构造器；空参的构造器的访问权限足够，通常问public。



### 调用运行时类的指定结构

**调用指定属性**

~~~java
Class clazz = Person.class;
//创建运行时类的对象
Person p = (Person) clazz.newInstance();

//1. getDeclaredField(String fieldName):获取运行时类中指定变量名的属性
Field name = clazz.getDeclaredField("name");
//2. 保证当前属性是可访问的
name.setAccessible(true);
//3. 获取、设置指定对象的此属性值
name.set(p, "Tom");

System.out.println(name.get(p));
~~~

**调用指定的方法：**

~~~java
Class clazz = Person.class;
//创建运行时类的对象
Person p = (Person) clazz.newInstance();

//1. 获取指定的某个方法
//getDeclaredMethod():参数1：指明获取的方法的名称  参数2：指明获取的方法的形参列表
Method show = clazz.getDeclaredMethod("show", String.class);
//2. 当前方法可访问的
show.setAccessible(true);
//3. invoke():参数1：方法调用者  参数2：给方法形参赋值的实参
//invoke()返回值即为对应类中调用的方法的返回值
Object nation = show.invoke(p, "CHN");

System.out.println(nation);
~~~

**调用指定构造器：**

~~~java
Class clazz = Person.class;

//private Person(String name)
//1. 获取指定的构造器
clazz.getDeclaredConstructor()：参数：指明形参列表
Constructor constructor = clazz.getDeclaredConstructor(String.class);
//2. 保证构造器可访问
constructor.setAccessible(true);
//3. 调用此构造器创建运行时类的对象
Person p = (Person) constructor.newInstance("Tom");

System.out.println(p);
~~~



### 动态代理





## 十一、其它类

### 日期类

日期类有两类，一个是用来表示时间点的**Date**类；另一个是用大家熟悉的日历表示法表示日期的**LocalDate**类



**LocalDate类**

**类的创建**

~~~java
//会创建当前日期
LocalDate today = LocalDate.now();
//可以提供年、月、日来创建
LocalDate date = LocalDate.of(1999,12,10);
~~~

**常用API**

~~~java
//可以增加天数得到新的日期
LocalDate nextMonthDay = today.plusDays(31);
year = today.getYear();
month = today.getMonthValue();
day = today.getDayOfMonth();
~~~

