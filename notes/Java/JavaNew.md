* [Java8](#java8)
    * [Lambda](#lambda)
    * [函数式接口](#函数式接口)
    * [方法引用与构造器引用](#方法引用与构造器引用)
    * [强大的StreamAPI](#强大的streamapi)
    * [Optional类](#optional)
    * [其他](#其他)

## Java8

Java8是Java语言开发的一个主要版本。

Java8是2014年发布的。可以看成是自Java5以来最具革命性的版本。

主要体现在`Lambda`和`StreamAPI`



**优点**

* 速度更快
* 代码更少(增加了新的语法：`Lambda表达式`)
* 强大的`Stream API`

* 便于并行
* 最大减少空指针异常：Optional
* Nashorn引擎，允许在JVM上运行JS应用



### Lambda

**格式：**

`(形参列表) -> 方法体`



**本质：**

lambda表达式的本质：作为接口的实例。【函数式接口】



**使用(6种情况)**：

* **无参无返回值**

  ~~~java
  //原先写法
  Runnable r1 = new Runnable() {
      @Override
      public void run() {
          System.out.println("我爱北京天安门");
      }
  };
  
  //lambda写法
  Runnable r2 = () -> {
      System.out.println("lambda我爱北京天安门");
  };
  ~~~

* **有参无返回值**

  ~~~java
  //原先写法
  Consumer<String> con = new Consumer<String>() {
      @Override
      public void accept(String t) {
          System.out.println(t);
      }
  };
  
  //lambda写法
  Consumer<String> con1 = (String s) -> {
      System.out.println(s);
  };
  ~~~

* **数据类型可以省略，因为可以由编译器推断得出，称为“类型推断”**

  ~~~java
  Consumer<String> con2 = (s) -> {
      System.out.println(s);
  };
  ~~~

* **lambda若只需要一个参数时，参数的小括号可以省略**

  ~~~java
  Consumer<String> con3 = s -> {
      System.out.println(s);
  };
  ~~~

* **Lambda需要两个或以上的参数，多条执行语句，并且可以由返回值**

  ~~~java
  //原先
  Comparator<Integer> com1 = new Comparator<Integer>() {
      @Override
      public int compare(Integer o1, Integer o2) {
          System.out.println(o1);
          System.out.println(o2);
          return o1.compareTo(o2);
      }
  };
  
  //lambda写法
  Comparator<Integer> com2 = (o1, o2) -> {
      System.out.println(o1);
      System.out.println(o2);
      return o1.compareTo(o2);			
  };
  ~~~

* **当lambda体只有一条语句时，return与大括号若有，都可以省略**

  ~~~java
  Comparator<Integer> com3 = (o1, o2) -> o1.compareTo(o2);
  ~~~



**总结**

> ->左边：lambda形参列表的参数类型可以省略(类型推断)；如果lambda形参列表只有一个参数，其一对()可以省略
>
> ->右边：lambda体应该使用一对{}包裹；如果lambda体只有一条执行语句(可能是return语句)，省略{}与return。



### 函数式接口

如果一个接口中，只声明了一个抽象方法，则此接口就称为函数式接口。

可以增加注释`@FunctionalInterface`【用来检验是否是函数式接口】。

使用情景：**实现函数式接口的匿名对象的时候可以使用lambda**。



**Java内置四大核心函数式接口**

| 名字       | API            | 类型              |
| ---------- | -------------- | ----------------- |
| 消费型接口 | Consumer<T>    | void accpet(T t)  |
| 供给型接口 | Supplier<T>    | T get()           |
| 函数型接口 | Function<T, R> | R apply(T t)      |
| 断定型接口 | Predicate<T>   | boolean test(T t) |



应用举例：

~~~java
public static void main(String[] args) {
    happyTime(500, new Consumer<Double>() {
        public void accept(Double aDouble) {
            System.out.println("学习太累了，去天上人间买了瓶矿泉水");
        }
    });

    happyTime(400, money -> System.out.println("学习太累了，去天上人间买了瓶矿泉水,价格为" + money));

    System.out.println("*******************************");

    List<String> list = Arrays.asList("北京","南京","天京","天津");

    List<String> filterStr = filterString(list, new Predicate<String>() {
        public boolean test(String s) {
            return s.contains("京");
        }
    });
    System.out.println(filterStr);

    System.out.println("*******************************");

    List<String> filterStr1 = filterString(list, s-> s.contains("京"));

    System.out.println(filterStr1);
}

public static void happyTime(double money, Consumer<Double> con) {
    con.accept(money);
}

//根据给定的规则，过滤集合中的字符串。此规则由Predicate的方法决定
public static List<String> filterString(List<String> list, Predicate<String> pre) {
    ArrayList<String> filterList = new ArrayList<>();

    for(String s : list) {
        if(pre.test(s)) {
            filterList.add(s);
        }
    }
    return filterList;
}
~~~



### 方法引用与构造器引用

1. **使用情景**：当要传递给Lambda体的操作，已经有实现的方法了，可以使用方法引用！

2. **本质**：本质上是lambda表达式。

3. **使用格式**：    类(对象)    ::    方法名

4. 具体分为如下三种情况：

   对象 :: 非静态方法

   类 :: 静态方法

   类 :: 非静态方法



* 情况一：对象 :: 实例方法

  ~~~java
  //lambda
  Consumer<String> con1 = str -> System.out.println(str);
  con1.accept("北京");
  
  //对象引用
  Consumer<String> con2 = System.out::println;
  con1.accept("南京");
  ~~~

    ~~~java
    //lambda
    Employee emp = new Employee(1001, "Tom", 23, 5600);
    Supplier<String> sup1 = () -> emp.getName();
    System.out.println(sup1.get());
    
    //对象引用
    Supplier<String> sup2 = emp::getName;
    System.out.println(sup2.get());
    ~~~

* 情况二：类 :: 静态方法

    ~~~java
    //Comparator中的int compare(T t1, T t2)
    //Integer   中的int compare(T t1, T t2)
    
    //lambda
    Comparator<Integer> com1 = (t1, t2) -> Integer.compare(t1, t2);
    System.out.println(com1.compare(12, 1));
    
    //方法引用
    Comparator<Integer> com2 = Integer::compare;
    System.out.println(com2.compare(12, 33));
    ~~~

    ~~~java
    //lambda
    Function<Double, Long> fun1 = d -> Math.round(d);
    //方法引用
    Function<Double, Long> fun2 = Math::round;
    ~~~

* 情况三：类 :: 非静态方法(难)

    ~~~java
    Comparator<String> com3 = (s1, s2) -> s1.compareTo(s2);		
    System.out.println(com3.compare("abc", "abc"));
    
    Comparator<String> com4 = String :: compareTo;
    System.out.println(com4.compare("abcd", "abc"));
    ~~~

    ~~~java
    BiPredicate<String, String> pre1 = (s1, s2) -> s1.equals(s2);
    System.out.println(pre1.test("abc", "abdd"));
    
    BiPredicate<String, String> pre2 = String::equals;
    System.out.println(pre2.test("abc", "abd"));
    ~~~

    ~~~java
    Employee employee = new Employee(1001, "Jerry", 23, 6000);
    Function<Employee, String> func1 = e -> e.getName();
    System.out.println(func1.apply(employee));
    
    Function<Employee, String> func2 = Employee::getName;
    System.out.println(func2.apply(employee));
    ~~~



**构造器引用**

类似于方法引用

~~~java
Supplier<Employee> sup1 = () -> new Employee();
System.out.println(sup1.get());

Supplier<Employee> sup2 = Employee :: new;
System.out.println(sup2.get());
~~~

~~~java
Function<Integer, Employee> func1 = id -> new Employee(id);
Employee employee = func1.apply(1001);

Function<Integer, Employee> func2 = Employee :: new;
Employee employee2 = func2.apply(1002);
~~~

~~~java
BiFunction<Integer,String, Employee> func3 = (id, name) -> new Employee(id, name);
System.out.println(func3.apply(1001, "Tom"));

BiFunction<Integer,String, Employee> func4 = Employee :: new;
System.out.println(func4.apply(1002, "Jack"));
~~~



**数组引用**

可以将数组看作特殊的类。

~~~java
Function<Integer, String[]> func5 = length -> new String[length];
String[] arr1 = func5.apply(5);
System.out.println(Arrays.toString(arr1));

Function<Integer, String[]> func6 = String[] :: new;
String[] arr2 = func6.apply(6);
System.out.println(Arrays.toString(arr2));
~~~



### 强大的stream API

Java8中两大最重要的改变。第一个是`Lambda表达式`；另一个则是`Stream API`。

让代码高效、干净、整洁。

StreamAPI是什么？`集合讲的是数据，Stream讲的是计算！`



**注意**

* Stream自己不会存储元素。
* Stream不会改变源对象，相反，他们会返回一个持有结果的新Stream。
* Stream操作时延迟执行的。这意味着他们会等到需要结果的时候才执行。



**执行步骤**

1. 创建Stream

   > 一个数据源(如：集合、数组)，获取一个流

2. 中间操作【筛选、切片、映射、排序】

   > 一个中间操作链，对数据源的数据进行处理

3. 终止操作【匹配和查找、规约收集】

   > 一旦执行终止操作，**就执行中间操作**，并产生结果。之后不会再使用



**创建Stream**

* **创建Stream方式一：通过集合**

    ~~~java
    List<String> strings = new LinkedList<>();
    strings.add("aaa");
    strings.add("bbb");
    strings.add("ccc");
    
    //default Stream<E> stream():  返回一个顺序流
    Stream<String> stream = strings.stream();
    
    //default Stream<E> parallelStream(): 返回一个并行流
    Stream<String> parallelStream = strings.parallelStream();
    ~~~

* **方式二：通过数组创建**

    ~~~java
    int[] arr = new int[]{1,2,3,4,5,6};
    //调用Arrays类的static<T> Stream<T> stream(T[] arrays):返回一个流
    IntStream stream = Arrays.stream(arr);
    
    String[] strings = new String[]{"A,C", "DDD", "ABC"};
    Stream<String> stream1 = Arrays.stream(strings);
    ~~~

* **方式三：通过Stream的of()**

    ~~~java
    Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6);
    ~~~

* 方式四：创建无限流

    ~~~java
    //迭代
    //public static<T> Stream<T> iterator(final T seed, final UnaryOperator<T> f)
    
    //遍历前10个偶数
    Stream.iterate(0,t -> t + 2).limit(10).forEach(System.out::println);
    
    //生成
    //public static<T> Stream<T> generate(Supplier<T> s)
    Stream.generate(Math::random).limit(10).forEach(System.out::println);
    ~~~



**Stream的中间操作**

1. **筛选与切片**

    ~~~java
    List<Integer> list = new LinkedList<>();
    list.add(12000);
    list.add(8000);
    list.add(5000);
    list.add(10000);
    list.add(9000);
    list.add(9000);
    list.add(25000);
    //filter(Predicate p)--接受Lambda，从流中排除某些元素
    Stream<Integer> stream = list.stream();
    stream.filter(i -> i > 7000).forEach(System.out::println);
    
    System.out.println();
    
    //limit(n)--截断流，使其元素不超过给定数量,若流中元素不足n个，则返回空
    list.stream().limit(2).forEach(System.out::println);
    
    System.out.println();
    //skip(n)--跳过元素，返回
    list.stream().skip(3).forEach(System.out::println);
    
    System.out.println();
    //distinct()--筛选，去重
    list.stream().distinct().forEach(System.out::println);
    ~~~

2. **映射**

    ~~~java
    //映射
    @Test
    public void test2(){
        //map(Function f)
        List<String> list = Arrays.asList("aa", "bb", "cc", "dd");
        list.stream().map(str -> str.toUpperCase()).forEach(System.out::println);//aa bb cc dd
    
        //flatMap(Function f)：将流中的每个值都换成另一个流，然后把所有的流连起来,类似于addAll
        Stream<Character> characterStream = list.stream().flatMap(StreamAPITest1::fromStringToStream);
        characterStream.forEach(System.out::println);//a a b b c c d d
    }
    
    //将字符串中的多个字符构成的集合转换为Stream的实例
    public static Stream<Character> fromStringToStream(String str){
        ArrayList<Character> list = new ArrayList<>();
        for(Character c : str.toCharArray()){
            list.add(c);
        }
        return list.stream();
    }
    ~~~

3. **排序**

   ~~~java
   //sorted()--自然排序
   List<Integer> list = Arrays.asList(12, 34, 54, -77, 2, 334);
   list.stream().sorted().forEach(System.out::println);
   
   System.out.println();
   
   //sorted(Comparator com)--按比较器顺序排序
   List<String> stringList = Arrays.asList("aaa", "abc", "dd", "zzz");
   stringList.stream().sorted((s1, s2) -> -s1.compareTo(s2)).forEach(System.out::println);
   ~~~



**终止操作**

1. 匹配与查找

   ~~~java
   List<Integer> list = Arrays.asList(120, 33, 55, 20, 30);
   //allMatch(Predicate p)--检查是否匹配所有的元素
   boolean flag = list.stream().allMatch(x -> x > 18);
   System.out.println(flag);
   
   //anyMatch(Predicate p)--检查是否至少匹配一个元素
   boolean anyMatch = list.stream().anyMatch(x -> x < 20);
   System.out.println(anyMatch);
   
   //noneMath(Predicate p)--检查是否没有的匹配的元素
   boolean noneMatch = list.stream().noneMatch(x -> x > 1000);
   System.out.println(noneMatch);
   
   //findFirst--返回第一个元素
   Optional<Integer> first = list.stream().findFirst();
   System.out.println(first);
   
   //findAny--返回当前流中任意的元素
   Optional<Integer> any = list.stream().findAny();
   System.out.println(any);
   
   //count--返回流中元素的个数
   long count = list.stream().count();
   System.out.println(count);
   
   //max(Comparator c)--返回流中的最大值
   Optional<Integer> max = list.stream().max(Integer::compare);
   System.out.println(max);
   
   //min(Comparator c)--返回流中的最小值
   Optional<Integer> min = list.stream().min(Integer::compare);
   System.out.println(min);
   
   //forEach(Consumer c)--内部迭代
   list.stream().forEach(System.out::println);
   ~~~

2. 归约

   ~~~java
   //reduce(T identity, BinaryOperator)--可以将流中元素结合起来，得到一个值
   List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
   Integer reduce = list.stream().reduce(0, Integer::sum);
   System.out.println(reduce);
   
   //reduce(BinaryOperator)--返回Optional<T t>
   Optional<Integer> reduce1 = list.stream().reduce(Integer::sum);
   System.out.println(reduce1);
   ~~~

3. 收集

   ~~~java
   //collect(Collector c)--将流转换为其他形式
   List<Integer> list = Arrays.asList(11, 12, 33, 55,342, 22, 11);
   
   Set<Integer> collect = list.stream().collect(Collectors.toSet());
   System.out.println(collect);
   ~~~



**Collectors的一些常用API**

| 方法         | 返回类型      | 作用                       |
| ------------ | ------------- | -------------------------- |
| toList       | List<T>       | 把流中元素收集到List       |
| toSet        | Set<T>        | 把流中元素收集到Set        |
| toCollection | Collection<T> | 把流中元素收集到创建的集合 |



### Optional类

**空指针异常**是导致Java应用程序失败的常见原因。

Optional类用来避免空指针异常。



**创建Optional类对象的方法**

~~~java
Optional.of(T t)			//创建一个Optional实例，t必须非空
Optional.empty()			//创建一个空的Optional实例
Optional.ofNullable(T t)	//t可以为null
~~~

**判断Optional容器中是否包含对象**

~~~java
boolean isPresent()			//判断是否包含对象
void ifPresent(Consumer<? super T> consumer)	//如果有值，就执行Consumer接口的实现代码，并且该值会作为参数传给它
~~~

**获取Optional容器的对象**

~~~java
T get()				//如果调用对象包含之，则返回该值，否则抛异常
T orElse(T other)	//如果有值则将其返回，否则返回other
T orElseGet(Supplier<? extends T> other)	//如果有值则返回，否则返回Supplier接口实现的提供的对象
T orElseThrow(Supplier<? extends X> exceptionSupplier)	//如果有值则将其返回，否则抛出Supplier接口实现提供的异常
~~~



使用例子

~~~java
Girl girl = new Girl();
girl = null;

//ofNullable(T t): t可以为null
Optional<Girl> optionalGirl = Optional.ofNullable(girl);
System.out.println(optionalGirl);

//orElse(T t):如果当前的Optional内部封装的t是非空的，则返回t
//如果t是空的,则返回备胎("赵丽颖")
Girl girl1 = optionalGirl.orElse(new Girl("赵丽颖"));
System.out.println(girl1);
~~~



常用搭配：

> Optional.of(T t)	与	T get()
>
> Optional.empty()	与	T orElse(T other)



### 其他

**并行流/串行流**

运行效率更快



**jjs**

js代码可以在Java虚拟机上运行.

> jjs func.js