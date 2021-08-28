* JavaSE
  * <a href="#零.环境搭建">零.环境搭建</a>
  * <a href="#一.基础知识">一.基础知识</a>

## <div id = "">零.环境搭建</div>


JDK：编写Java程序的程序员使用的软件

JRE：运行Java程序的用户使用的软件

**安装顺序**

下载JDK，并为之设置环境变量

> 下载完成之后在cmd中运行javac --version查看版本

**使用命令行工具**

> javac Hello.java
>
> java Hello

javac是一个Java编译器，它将文件Hello.java编译成Hello.class。

java程序启动Java虚拟机，虚拟机执行编译器编译到类文件中的字节码。

## <div id = "一.基础知识">一.基础知识</div>

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



### 大数

在整数和浮点数精度不能够满足需求，那么可以使用`BigInteger`和`BigDecimal`这两个类。他们可以实现任意精度的运算。



运算的时候需要使用类中的相应方法，并将其中的类型使用`xxx.valueOf(a)`转换为响应的类型

~~~java
BigInteger d = c.mutiply(b.add(BigInteger.valueOf(2)));//d = c * 2;
~~~



### 数组

**声明**

~~~java
int[] a;
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

