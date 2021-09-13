* [Java容器](#java容器)
    * [一、概述](#一概述)
        * [Collection](#collection)
        * [Map](#map)
    * [二、容器中的设计模式](#二容器中的设计模式)
        * [迭代器模式](#迭代器模式)
        * [适配器模式](#适配器模式)
    * [三、源码分析](#三源码分析)
        * [ArrayList](#arraylist)
        * [Vector](#vector)
        * [LinkedList](#linkedlist)
        * [HashMap](#hashmap)
        * [LinkedHashMap](#linkedhashmap)

# Java容器

## 一、概述

容器主要包括 Collection 和 Map 两种，Collection 存储着对象的集合，而 Map 存储着键值对（两个对象）的映射表。

### Collection

~~~java
|----Collection接口
	|----List接口
		|----ArrayList
		|----Vector
    	|----LinkedList

	|----Set接口
    	|----TreeSet
		|----HashSet
    	|----LinkedHashSet
    
    |----Queue接口
    	|----LinkedList
    	|----PriorityQueue
~~~

1. **List**
    * ArrayList：基于动态数组实现，支持随机访问
    * Vector：和ArrayList类似，但它是线程安全的
    * LinkedList：基于双向链表实现，只能顺序访问，但可以快速的在链表中插入和删除元素。
2. **Set**
    * TreeSet：基于红黑树实现，支持有序性操作。(例如根据一个范围查找元素的操作。但是效率不如HashSet，HashSet查找的时间复杂度为O(1)，TreeSet则为O(logN))。
    * HashSet：基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用Iterator遍历HashSet得到的结果是不确定的。
    * LinkedHashSet：具有HashSet的查找效率，并且内部使用双向链表维护元素的插入顺序。
3. **Queue**
    * LinkedList：可以用它来实现双向队列。
    * PriorityQueue：基于堆结构实现，可以用它来实现优先队列。

### Map

~~~java
|----Map接口
    |----TreeMap
	|----HashMap
	    |----LinkedHashMap
    |----Hashtable
    	|----Properties
~~~

- TreeMap：基于红黑树实现。
- HashMap：基于哈希表实现。可以实现键值为null。
- HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程同时写入 HashTable 不会导致数据不一致。它是遗留类，不应该去使用它，而是使用ConcurrentHashMap 来支持线程安全，ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。不能实现键值为null。
- LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。

## 二、容器中的设计模式

### 迭代器模式

Collection继承了Iterator接口，其中iterator()方法能够产生一个Iterator对象，通过这个对象就可以迭代遍历Collection中的元素。

从JDK1.5之后可以使用**foreach**方法来遍历实现Iterable接口的聚合对象。

~~~java
List<String> list = new ArrayList<>();
list.add("a");
list.add("b");
for (String item : list) {
    System.out.println(item);
}
~~~

**遍历**的另外一种实现。

~~~java
Iterator iterator = coll2.iterator();

iterator = coll2.iterator();
while(iterator.hasNext()){
    System.out.println(iterator.next());
}
~~~

**remove()方法的使用**：

~~~java
Collection coll2 = new ArrayList();
coll2.add(123);
coll2.add(new String("123333"));
coll2.add("12aaa");
coll2.add(false);

Iterator iterator = coll2.iterator();
while(iterator.hasNext()){
    Object obj = iterator.next();
    if("12aaa".equals(obj)){
        iterator.remove();
    }
}
~~~

### 适配器模式

java.util.Arrays#asList() 可以把数组类型转换为 List 类型。

应该注意的是 asList() 的参数为泛型的变长参数，不能使用基本类型数组作为参数，只能使用相应的包装类型数组。

```java
Integer[] arr = {1, 2, 3};
List list = Arrays.asList(arr);
```

也可以使用以下方式调用 asList()：

```java
List list = Arrays.asList(1, 2, 3);
```



## 三、源码分析

在 IDEA 中 double shift 调出 Search EveryWhere，查找源码文件，找到之后就可以阅读源码。

### ArrayList

1. **概述**

   因为ArrayList是基于数组实现的，所以支持快速随机访问。

   RandomAccess接口标识着该类支持快速随机访问。

   ~~~java
   public class ArrayList<E> extends AbstractList<E>
           implements List<E>, RandomAccess, Cloneable, java.io.Serializable
   ~~~

   数组的默认大小为10。

   private static final int DEFAULT_CAPACITY = 10;

  
2. **扩容**

   添加元素时使用 ensureCapacityInternal() 方法来保证容量足够，如果不够时，需要使用 grow() 方法进行扩容，新容量的大小为 `oldCapacity + (oldCapacity >> 1)`，即 oldCapacity+oldCapacity/2。其中 oldCapacity >> 1 需要取整，所以**新容量大约是旧容量的 1.5 倍左右**。（oldCapacity 为偶数就是 1.5 倍，为奇数就是 1.5 倍-0.5）

   扩容操作需要调用 `Arrays.copyOf()` 把原数组整个复制到新数组中，这个操作代价很高，因此最好在创建 ArrayList 对象时就指定大概的容量大小，减少扩容操作的次数。

    ~~~java
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
    
    private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        ensureExplicitCapacity(minCapacity);
    }
    
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
    
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    ~~~

3. **删除元素**

   需要调用 System.arraycopy() 将 index+1 后面的元素都复制到 index 位置上，该操作的时间复杂度为 O(N)，可以看到 ArrayList 删除元素的代价是非常高的。

    ~~~java
    public E remove(int index) {
        rangeCheck(index);
        modCount++;
        E oldValue = elementData(index);
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index, numMoved);
        elementData[--size] = null; // clear to let GC do its work
        return oldValue;
    }
    ~~~

4. jdk8相比7的更改

   创建的时候初始化为{}，并没有创建长度为10，直到第一次调用add()时，底层才创建了长度为10的数组。

    ```java
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
    
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    ```

   jdk7中ArrayList的创建类似于单例模式中的饿汉式，而jdk8中对象的创建类似于懒汉式。节省内存。

### Vector

1. **同步**

   它的实现与 ArrayList 类似，但是使用了 synchronized 进行同步。

    ~~~java
    public synchronized boolean add(E e) {
        modCount++;
        ensureCapacityHelper(elementCount + 1);
        elementData[elementCount++] = e;
        return true;
    }
    
    public synchronized E get(int index) {
        if (index >= elementCount)
            throw new ArrayIndexOutOfBoundsException(index);
    
        return elementData(index);
    }
    ~~~

2. **扩容**

   Vector 的构造函数可以传入 capacityIncrement 参数，它的作用是在扩容时使容量 capacity 增长 capacityIncrement。如果这个参数的值小于等于 0，扩容时每次都令 capacity 为原来的两倍。

    ```java
    public Vector(int initialCapacity, int capacityIncrement) {
        super();
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        this.elementData = new Object[initialCapacity];
        this.capacityIncrement = capacityIncrement;
    }
    ```

    ```java
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    ```

   调用没有 capacityIncrement 的构造函数时，capacityIncrement 值被设置为 0，也就是说默认情况下 Vector 每次扩容时容量都会翻倍。

3. **与ArrayList的比较**

    * Vector 是同步的，因此开销就比 ArrayList 要大，访问速度更慢。最好使用 ArrayList 而不是 Vector，因为同步操作完全可以由程序员自己来控制；
    * Vector 每次扩容请求其大小的 2 倍（也可以通过构造函数设置增长的容量），而 ArrayList 是 1.5 倍。

4. **替代方法**

   可以使用 `Collections.synchronizedList();` 得到一个线程安全的 ArrayList。

    ```java
    List<String> list = new ArrayList<>();
    List<String> synList = Collections.synchronizedList(list);
    ```

   也可以使用 concurrent 并发包下的 CopyOnWriteArrayList 类。

   ```java
   List<String> list = new CopyOnWriteArrayList<>();
   ```

### LinkedList

1. **概述**

   基于双向链表实现，使用Node存储链表节点的信息。

    ```java
    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;
    }
    ```

   每个链表存储了 first 和 last 指针：

    ```java
    transient Node<E> first;
    transient Node<E> last;
    ```

2. **与ArrayList的比较**

   ArrayList 基于动态数组实现，LinkedList 基于双向链表实现。ArrayList 和 LinkedList 的区别可以归结为数组和链表的区别：

    - 数组支持随机访问，但插入删除的代价很高，需要移动大量元素；
    - 链表不支持随机访问，但插入删除只需要改变指针。



### HashMap

1. **存储结构**

   内部包含了一个 Entry 类型的数组 table。Entry 存储着键值对。它包含了四个字段，从 next 字段我们可以看出 Entry 是一个链表。即数组中的每个位置被当成一个桶，一个桶存放一个链表。HashMap 使用拉链法(7头8尾)来解决冲突，同一个链表中存放哈希值和散列桶取模运算结果相同的 Entry。

   ```java
   transient Entry[] table;
   ```

    ```java
    static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;
        int hash;
    
        Entry(int h, K k, V v, Entry<K,V> n) {
            value = v;
            next = n;
            key = k;
            hash = h;
        }
    }
    ```

   在实例化以后，底层创建了长度是16的一维数组Entry[] table。

2. **put**

   > 首先，调用key1所在类的hashCode()计算key1的哈希值，此哈希值经过某种算法以后，得到Entry数组中的存放位置
   >
   > 如果此位置上的数据为空，此时的key1-value1添加成功  ---情况1
   >
   > 如果此位置上的数据不为空，（意味着此位置上存在着一个或多个数据（以链表形式存在）），比较key1和已经存在的一个或多个数据的哈希值：
   >
   > ​			如果key1的哈希值与已经存在的数据的哈希值不同，此时key1-value1添加成功. ---情况2
   >
   > ​			如果key1的哈希值与已经存在的数据（key2-value2）的哈希值相同，继续比较：调用key1所在类的equals()方法，比较：
   >
   > ​						如果equals()返回false:此时的key1-value1添加成功.   ---情况3
   >
   > ​						如果equals()返回true：使用value1替换相同key的value值。

   > 补充：关于情况2和情况3：此时key1-value1和原来的数据以链表的方式存储。
   >
   > 在不断的添加的过程中，会涉及到扩容问题，默认的扩容方式：扩容为原来的2倍，并将原有的数据复制过来。

3. jdk8中相较于jdk7的不同

   > 1. new HashMap():底层没有创建一个长度为16的数组
   >
   > 2. jdk 8底层的数组是：Node[],而非Entry[]
   >
   > 3. 首次调用put()方法是，底层创建长度为16的数组
   >
   > 4. jdk7底层结构只有：数组+链表，jdk8中底层结构：数组+链表+红黑树
        >
        >    当数组的某一个索引位置上的元素以链表形式存在的数据个数 > 8 且当前数据的长度 > 64时，此时此索引位置上的所有数据修改为红黑树存储。

   > DEFAULT_INITIAL_CAPACITY：HashMap的默认容量：16
   >
   > DEFAULT_LOAD_FACTOR：HashMap的默认记载因子：0.75
   >
   > threshold：扩容的临界值： = 容量 * 填充因子：16 * 0.75 => 12
   >
   > TREEIFY_THRESHOLD：Bucket中链表长度大于该默认值，转化为红黑树
   >
   > MIN_TREEIFY_CAPACITY：桶中的Node被树化时最小的hash表容量:64

### LinkedHashMap

1. **存储结构**

   继承自 HashMap，因此具有和 HashMap 一样的快速查找特性。

~~~java
static class Entry<K,V> extends HashMap.Node<K,V> {
	Entry<K,V> before, after;
    Entry(int hash, K key, V value, Node<K,V> next) {
    	super(hash, key, value, next);
    }
}
~~~

