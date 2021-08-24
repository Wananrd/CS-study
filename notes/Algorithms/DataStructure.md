* [数据结构](#anchor)
  * 链表与邻接表
  * [栈与队列](#anchor2)
  * KMP
  * Trim

# <span id = "anchor">数据结构</span>


## 链表与邻接表

~~~mermaid
graph LR
A(链表a)-->B1(动态链表:面试aaa)
A(链表a)-->B2(静态链表:笔试aaa)
B1-->C1(缺点a)
C1-->D1(开辟慢aa)
B2-->E1(优点a)
B2-->E2(单链表:邻接表aaa)
B2-->E5(双链表aa)
E2-->E3(树a)
E2-->E4(图a)
E1-->F1(实现方便)
~~~

### 单链表、邻接表

单链表代码

~~~cpp
// head 表示头结点的下标
// e[i] 表示结点i的值
// ne[i] 表示结点i的next指针是多少
// idx 存储当前已经用到了哪个点
int head, e[N], ne[N], idx;

void init()// 初始化
{
    head = -1;
    idx = 0;
}

void add_to_head(int x)//  将x插到头结点
{
    e[idx] = x;
    ne[idx] = head;
    head = idx;
    idx ++;
}

void remove(int k){//  将下标是k的点后面的点删除
    ne[k] = ne[ne[k]];
}
~~~



### 双链表

~~~cpp
// e[]表示节点的值，l[]表示节点的左指针，r[]表示节点的右指针，idx表示当前用到了哪个节点
int e[N], l[N], r[N], idx;

// 初始化
void init()
{
    //0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;
}

// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}

// 删除节点a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}
~~~

## <span id = "anchor2">栈与队列</span>

**栈**

~~~cpp
// tt表示栈顶
int stk[N], tt = 0;

// 向栈顶插入一个数
stk[ ++ tt] = x;

// 从栈顶弹出一个数
tt -- ;

// 栈顶的值
stk[tt];

// 判断栈是否为空
if (tt > 0)
{

}
~~~

**队列(普通队列)**

~~~cpp
// hh 表示队头，tt表示队尾
int q[N], hh = 0, tt = -1;

// 向队尾插入一个数
q[ ++ tt] = x;

// 从队头弹出一个数
hh ++ ;

// 队头的值
q[hh];

// 判断队列是否为空
if (hh <= tt)
{

}
~~~

**循环队列**

~~~cpp
// hh 表示队头，tt表示队尾的后一个位置
int q[N], hh = 0, tt = 0;

// 向队尾插入一个数
q[tt ++ ] = x;
if (tt == N) tt = 0;

// 从队头弹出一个数
hh ++ ;
if (hh == N) hh = 0;

// 队头的值
q[hh];

// 判断队列是否为空
if (hh != tt)
{

}
~~~



**单调栈**

~~~cpp
常见模型：找出每个数左边离它最近的比它大/小的数
int tt = 0;
for (int i = 1; i <= n; i ++ )
{
    while (tt && check(stk[tt], i)) tt -- ;
    stk[ ++ tt] = i;
}
~~~

**单调队列**

~~~cpp
常见模型：找出滑动窗口中的最大值/最小值
int hh = 0, tt = -1;
for (int i = 0; i < n; i ++ )
{
    while (hh <= tt && check_out(q[hh])) hh ++ ;  // 判断队头是否滑出窗口
    while (hh <= tt && check(q[tt], i)) tt -- ;
    q[ ++ tt] = i;
}
~~~

## KMP

~~~cpp
// s[]是长文本，p[]是模式串，n是s的长度，m是p的长度
求模式串的Next数组：
for (int i = 2, j = 0; i <= m; i ++ )
{
    while (j && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j ++ ;
    ne[i] = j;
}

// 匹配
for (int i = 1, j = 0; i <= n; i ++ )
{
    while (j && s[i] != p[j + 1]) j = ne[j];
    if (s[i] == p[j + 1]) j ++ ;
    if (j == m)
    {
        j = ne[j];
        // 匹配成功后的逻辑
    }
}
~~~



## Trie树

高效地存储和查找字符串集合的数据机构

![image-20210608141128893](C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20210608141128893.png)

插入

~~~cpp
void insert(char str[]){    int p = 0;    for(int i = 0; str[i]; i ++)    {        int u = str[i] - '0';        if(!son[p][u]) son[p][u] = ++ idx;        p = son[p][u];    }    cnt[p] ++;}
~~~

查询

~~~cpp
int query(char str[])
{
    int p = 0;
    for(int i = 0; str[i]; i ++)
    {
        int u = str[i] - '0';
        if(!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}
~~~



## 并查集

1. 将两个集合合并
2. 查询两个元素是否在一个集合中



>  问题1：如何判断树根：if(p[x] == x)
>
>  问题2：如何求x的集合编号：while(p[x] != x) x = p[x];
>
>  问题3：如何合并两个集合：px是x的集合编号,py是y的集合编号  p[x] = y;



**并查集优化:路径压缩**

将路径上所有的点指向根节点



**初始化**

~~~cpp
for(int i = 1; i <= n; i ++) p[i] = i;
~~~

**查找(路径压缩)**

~~~cpp
int find(int x)//返回x的祖宗结点 + 路径压缩
{
    if(p[x] != x) p[x] = find(p[x]);
    return p[x];
}
~~~

**合并**

~~~cpp
void merge(int x, int y)
{
    int tx = find(x), ty = find(y);
    if(tx != ty) p[tx] = ty;
}
~~~



## 手写堆

**结构**：完全二叉树

**性质**：小根堆每个点小于等于左右儿子

**存储**：一维数组

**基本操作**：down：往下调整

​					up：向上调整

STL中堆就是优先队列

**所有操作**：

1. 插入一个数

   > heap[++ size] = x; up(size);

2. 求集合中min

   > heap[1];

3. 删除最小值：最后元素覆盖第一个元素，down一遍

   > heap[1] = heap[size--]; down(1)

4. 删除任意元素：把最后一个数覆盖第k个数，删除最后一个数，down/up一遍

   > heap[k] = heap[size --]; down(k); up(k);

5. 修改任意元素

   > heap[k] = x;up(k);down(k);

**细节**：下标从1开始



**down核心代码**

~~~cpp
void down(int u) {
    int t = u;
    if (u * 2 <= cnt && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= cnt && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t) {
        swap(h[u], h[t]);
        down(t);
    }
}
~~~



**up核心代码**

~~~cpp
void up(int u)
{
    while(u / 2 && h[u / 2] > h[u])
    {
        swap(h[u], h[u / 2]);
        u /= 2;
    }
}
~~~



## 哈希表

~~~mermaid
graph LR
A(哈希表)-->B1(存储结构)
B1-->C1(开放地址法)
B1-->C2(拉链法)
A-->B2(字符串哈希方式)
~~~

问题：1. x%mod(质数)

​			2. 冲突

添加

删除



### 拉链法

~~~cpp
int h[N], e[N], ne[N], idx;

// 向哈希表中插入一个数
void insert(int x)
{
    int k = (x % N + N) % N;
    e[idx] = x;
    ne[idx] = h[k];
    h[k] = idx ++ ;
}

// 在哈希表中查询某个数是否存在
bool find(int x)
{
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i])
        if (e[i] == x)
            return true;

    return false;
}
~~~



### 开放地址法

~~~cpp
const int N = 200003, null = 0x3f3f3f3f; 
int h[N];

int find(int x)
{
    int k = (x % N + N) % N;
    while(h[k] != null && h[k] != x)
    {
        k ++;
        if(h[k] == N) k = 0;
    }
    return k;
}
~~~



### 字符串哈希

将字符s以前缀方式映射

**注意**

1. 不能将字母映射成0

2. Rp足够好不存在冲突



核心思想：将字符串看成P进制数，P的经验值是131或13331，取这两个值的冲突概率低
小技巧：取模的数用2^64，这样直接用unsigned long long存储，溢出的结果就是取模的结果

~~~cpp
typedef unsigned long long ULL;
ULL h[N], p[N]; // h[k]存储字符串前k个字母的哈希值, p[k]存储 P^k mod 2^64

// 初始化
p[0] = 1;
for (int i = 1; i <= n; i ++ )
{
    h[i] = h[i - 1] * P + str[i];
    p[i] = p[i - 1] * P;
}

// 计算子串 str[l ~ r] 的哈希值
ULL get(int l, int r)
{
    return h[r] - h[l - 1] * p[r - l + 1];
}
~~~



## C++ STL

### vector

变长数组，思想：倍增

~~~
#include <vector>

定义:
vector<int> a;//定义一个空vector
vector<int> a(10);//定义一个长度为10的vector
vector<int> a(10,3);//定义一个长度为10的vector，其中每个元素为3
vector<int> a[10];

a.size();	//返回大小
a.empty();	//返回是否为空
a.clear();	//清空
a.front();	//返回第一个数
a.back();	//返回最后一个数
a.push_back();	//压入一个数
a.pop_back();	//压出一个数

迭代器:
a.begin();
a.end();

a[1];

遍历:
for(int i = 0; i < a.size(); i ++) cout << a[i] << " ";
for(aoto i = a.begin(); i != a.end(); i ++) cout << *i << " ";
for(auto x : a) cout << x << " ";

支持比较运算 : 按字典序排序
a < b
~~~



**倍增思想**

> 系统为某一程序分配空间时所需时间啊，与空间大小无关，与申请次数有关
>
> 最开始分配一个32空间，数组长度不够的时候copy过来，大小分配64(32*2)
>
> 时间O(1)



### pair

~~~
定义：
pair<int, string> p;

初始化:
p.make_pair(10,"yxc");
p = {20,"sdf"};

p.first;	//第一个元素
p.second;	//第二个元素

支持表示运算：按字典序，以first为第一关键字，以second为第二关键字


存储三个不同东西
pair<int, pair<int, int>>p;
~~~



### string

字符串

~~~
定义:
string a = "yxc";
string a;

s.size();/s.length()
s.empty();
s.clear();

a.substr(1,2);	//从第1位置开始，返回长度为2，若长度大于字符串长度，则输出到末尾为止
a.substr(1);	//从1位置到最后位置

a.c_str();	//返回存储的第一个地址

a += "dsa";
~~~



### queue,priority_queue

队列

~~~
#include<queue>

q.size();
q.empty();
q.push();	//向队尾插入一个元素
q.front();	//返回对头元素
q.back();	//返回队尾元素
q.pop();	//弹出队头元素

清空
q = queue<int>();	//直接构造~~~

优先队列

~~~cpp
#include<queue>

定义
priority_queue<int> heap;	//默认大根堆

q.push();	//向队尾插入一个元素
q.top();	//返回堆顶元素
q.pop();	//弹出队头元素

//建立大根堆
法1:插入-x
法2:定义小根堆
priority_queue<int, vector<int>, greater<int>> heap;
~~~

### stack

栈

~~~cpp
push()	//向栈顶插入一个元素
top()	//返回栈顶元素
pop()	//弹出栈顶元素
~~~



### deque

双端队列

~~~cpp
d.size();
d.empty();
d.clear();
d.front();
d.back();
d.push_back();/d.pop_back();
d.push_front();/d.pop_front();
d.begin();/d.end();
d[]
~~~



### set,map,multiset,multimap

基于平衡二叉树（红黑树）实现，动态维护一个有序序列

~~~
size();
empty();
clear();
begin()/end(); ++/--	返回前趋和后继		时间复杂度O(logn)

set/multiset:
    insert();	//插入一个数
    find();		//查找一个数
    cout();		//返回某一个数的个数
    erase();
        (1)输入的一个x，删除所有x
        (2)删除一个迭代器，删除这个迭代器
    lower_bound(x):返回大于等于x的最小的数的迭代器
    upper_bound(x):返回大于x的最小的数的迭代器

map/multimap
    insert();	//插入的数是一个pair
    erase();	//输入的pair或者迭代器
    find();
    []		时间复杂度O(logn)
    lower_bound(x):返回大于等于x的最小的数的迭代器
    upper_bound(x):返回大于x的最小的数的迭代器
~~~



### unordered_set,unordered_map,unordered_multiset,unordered_multimap

哈希表

> 和上面类似，增删查改时间复杂度为O(1)
>
> 不支持 lower_bound()/upper_bound()，迭代器的++ , --



### bitset

压位

> 1024 bool需要一个字节
>
> 1024 B = 1 KB
>
> 压位128B

~~~
bitset<10000> s;	//个数
~,&,|,^
>>,<<
==,!=
[]
count()		//返回有多少个1
any()		//判断是否只有有一个1
none()		//判断是否全为0

set()		//把所有位置1
set(k,v)	//把第k位置0
flip()		//等价于~
flip(k)		//把第k位取反
~~~





## 树

## 树的遍历

### 已知中序，后序，求先序遍历

**已知后序与中序输出前序（先序）：**
**后序：3, 4, 2, 6, 5, 1（左右根）**
**中序：3, 2, 4, 1, 6, 5（左根右）**
**分析：因为后序的最后一个总是根结点，令i在中序中找到该根结点，则i把中序分为两部分，左边是左子树，右边是右子树。因为是输出先序（根左右），所以先打印出当前根结点，然后打印左子树，再打印右子树。左子树在后序中的根结点为root – (end – i + 1)，即为当前根结点-(右子树的个数+1)。左子树在中序中的起始点start为start，末尾end点为i – 1.右子树的根结点为当前根结点的前一个结点root – 1，右子树的起始点start为i+1，末尾end点为end。**
**输出的前序应该为：1, 2, 3, 4, 5, 6（根左右）**

~~~c++
#include <cstdio>
using namespace std;
int post[] = {3, 4, 2, 6, 5, 1};
int in[] = {3, 2, 4, 1, 6, 5};
void pre(int root, int start, int end) {
    if(start > end) return ;
    int i = start;
    while(i < end && in[i] != post[root]) i++;
    printf("%d ", post[root]);
    pre(root - 1 - end + i, start, i - 1);
    pre(root - 1, i + 1, end);
}

int main() {
    pre(5, 0, 5);
    return 0;
}
~~~





