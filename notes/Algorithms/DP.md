* [动态规划](#动态规划)
  * [常用模型](#常用模型)
    * [背包问题](#背包问题)
      * [01背包](#01背包)
      * [完全背包](#完全背包)
        * [01背包与完全背包区别](#01背包与完全背包区别)
      * [多重背包](#多重背包)
         * [二进制优化](#二进制优化)
      * [分组背包](#分组背包)
  * [常用dp类型](#常用dp类型)
    * [线性dp](#线性dp)
    * [区间dp](#区间dp)
    * [计数dp](#计数dp)
    * [树型dp](#树型dp)
    * [记忆化搜索](#记忆化搜索)
* 总结
  * dp优化
  * dp通解

# 动态规划

## 常用模型

### 背包问题

#### 01背包

**题目描述**

有 N 件物品和一个容量是 V 的背包。每件物品只能使用一次。

第 i 件物品的体积是 vi，价值是 wi。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

**输入格式**

第一行两个整数，N，V，用空格隔开，分别表示物品数量和背包容积。

接下来有 N 行，每行两个整数 vi,wi，用空格隔开，分别表示第 i 件物品的体积和价值。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

0<N,V≤1000
0<vi,wi≤1000

**输出样例**

```
4 5
1 2
2 4
3 4
4 5
```

**输出样例**

```
8
```

**题目分析**

每件物品只能用1次

~~~
状态表示f(i,j):只从前i个物品中选，总体积 ≤ j
			属性:Max
状态计算:f(i,j)--[不含i|含i]
		
	不含i:从1~i中选，体积不超过j并且不选择第i个，恰好其实就是从1~i-1中选，且体积不超过j。即f(i-1,j)
	含i:从1~i中选，体积不超过j，并且要包含i。
		选择曲线救国策略:体积都减vi。即f(i-1,j-vi)+wi
~~~



**朴素解决**

~~~cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;
int n, m;
int f[N][N];
int w[N], v[N];

int main()
{
    cin >> n >> m;
    
    for(int i = 1; i <= n; i ++) cin >> v[i] >> w[i];
    
    for(int i = 1; i <= n; i ++)
        for(int j = 0; j <= m; j ++){
            f[i][j] = f[i - 1][j];
            if(j >= v[i]) f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]);
        }
    
    cout << f[n][m] << endl;
    return 0;
}
~~~



**空间优化**

可以发现虽然保存用了二维数组保存，但实际上使用的时候只使用当前一组和前面一组的数据。

如果直接去掉一维。

~~~cpp
for(int i = 1; i <= n; i ++)
        for(int j = 0; j <= m; j ++)
            if(j >= v[i]) f[j] = max(f[j], f[j - v[i]] + w[i]);
~~~

会出现一个问题，当前的值会根据前面值而改变，所以可以利用逆序求dp

~~~cpp
for(int i = 1; i <= n; i ++)
    for(int j = m; j >= v[i]; j --)
        f[j] = max(f[j], f[j - v[i]] + w[i]);
~~~



#### 完全背包

**题目描述**

有 N 种物品和一个容量是 V 的背包，每种物品都有无限件可用。

第 i 种物品的体积是 vi，价值是 wi。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

**输入格式**

第一行两个整数，N，V，用空格隔开，分别表示物品种数和背包容积。

接下来有 N 行，每行两个整数 vi,wi，用空格隔开，分别表示第 i 种物品的体积和价值。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

0<N,V≤1000
0<vi,wi≤1000

**输出样例**

```
4 5
1 2
2 4
3 4
4 5
```

**输出样例**

```
10
```



**题目分析**

每件物品有无限多个

~~~
状态表示f(i,j):前i个物品中，总体积不大于j的所有选法
			属性:max
状态计算:f(i,j) -- [0|1|2|3|……|k-1|k]
		选择曲线救国:f(i,j) = f(i-1,j-k*v[i])+k*w[i]
		即表达式为f(i,j) = min(f[i][j], f(i-1,j-k*v[i])+k*w[i])
~~~



**朴素解法**

~~~cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;
int n, m;
int v[N], w[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    
    for(int i = 1; i <= n; i ++) cin >> v[i] >> w[i];
    
    for(int i = 1; i <= n; i ++)
        for(int j = 0; j <= m; j++)
            for(int k = 0; k * v[i] <= j; k ++)
                f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
    
    cout << f[n][m] << endl;
    return 0;
}
~~~



**公式优化**

根据公式

~~~
f[i][j]=max(f[i-1][j-k*v[i]]+k*w[i])
~~~

推导

~~~
f[i,j]=max(f[i-1,j], f[i-1,j-v]+w,f[i-1,j-2v]+2w,f[i-1,j-3v]+3w……)
f[i,j-v]=max(        f[i-1,j-v],  f[i-1,j-2v]+w, f[i-1,j-3v]+2w……)

我们可以发现f[i,j-v] +w就是f[i-1,j-v]+w,f[i-1,j-2v]+2w,f[i-1,j-3v]+3w……

因此推出f[i,j]=max(f[i-1,j],f[i,j-v]+w)
~~~


因而可以将三维优化成二维

~~~cpp
for(int i = 1; i <= n; i ++)
	for(int j = 0; j <= m; j++){
        f[i][j] = f[i - 1][j];
        if(j >= v[i]) f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);
    }
~~~

**空间优化**

还可以进一步将二维转化为一维，直接删去前面一维就能够实现

~~~cpp
for(int i = 1; i <= n; i ++)
    for(int j = v[i]; j <= m; j++)
        f[j] = max(f[j], f[j - v[i]] + w[i]);
~~~



##### 01背包与完全背包区别

可以发现01背包与完全背包问题最终代码非常类似

**01背包**

~~~cpp
for(int i = 1; i <= n; i ++)
    for(int j = m; j >= v[i]; j --)
        f[j] = max(f[j], f[j - v[i]] + w[i]);
~~~

**完全背包**

~~~cpp
for(int i = 1; i <= n; i ++)
    for(int j = v[i]; j <= m; j++)
        f[j] = max(f[j], f[j - v[i]] + w[i]);
~~~

两者的区别仅仅在遍历的方向上不同

01背包逆序推导

完全背包正序推导



#### 多重背包

**题目描述**

有 N 种物品和一个容量是 V 的背包。

第 i 种物品最多有 si 件，每件体积是 vi，价值是 wi。

求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。
输出最大价值。

**输入格式**

第一行两个整数，N，V，用空格隔开，分别表示物品种数和背包容积。

接下来有 N 行，每行三个整数 vi,wi,si，用空格隔开，分别表示第 i 种物品的体积、价值和数量。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

0<N≤1000
0<V≤2000
0<vi,wi,si≤2000



**朴素解法**

~~~cpp
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 110;
int n, m;
int v[N], w[N], s[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i ++) cin >> v[i] >> w[i] >> s[i];
    
    for(int i = 1; i <= n; i ++){
        for(int j = 0; j <= m; j ++){
            for(int k = 0; k <= s[i] && k * v[i] <= j; k ++)
                f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
~~~

**公式优化**

思路是想通过完全背包问题的公式优化

~~~
f[i,j] = max(f[i-1,j],f[i-1,j-v]+w,f[i-1,j-2v]+2w,……,f[i-1,j-sv]+sw)
f[i,j-v] = max(		  f[i-1,j-v],  f[i-1,j-2v]+w ,……,f[i-1,j-sv]+(s-1)w, f[i-1,j-(s+1)v]+sw)
~~~

不能通过f[i,j-v]找到与f[i,j]联系，不能够通过此方式优化



##### 二进制优化

公式优化行不通，在选择物品的时候假如有2000件物品会从0~2000进行遍历，时间过长，可以从此处着手优化。

从选择物品之处进行优化。

~~~
例如：
s = 1023，需要从0~1023一个一个枚举？
可以将此打包考虑
分为10组1,2,4,……,512
将s打包成10组,10组数字可以凑出0~1023任何数字
因而接下来就可以对这10组数字进行01背包的解法

例如:
s = 200
1,2,4,8,16,32,64,73
~~~

**思路：**

将物品使用二进制优化打包后使用01背包直接求解

**代码**

~~~cpp
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 25000;
int n,m;
int v[N], w[N], s[N];
int f[N];

int main(){
    cin >> n >> m;
    int cnt = 0;
    for(int i =1; i <= n; i++){//二进制优化核心
        int a, b, s;
        cin >> a >> b >> s;
        int k = 1;
        while(k <= s){
            cnt ++;
            v[cnt] = a * k;
            w[cnt] = b * k;
            s -= k;
            k *= 2;
        }
        if(s > 0){
            cnt ++;
            v[cnt] = a * s;
            w[cnt] = b * s;
        }
    }
    n = cnt;
    for(int i = 1; i <= n; i ++){//01背包
        for(int j = m; j >= v[i]; j --){
            f[j] = max(f[j], f[j - v[i]] + w[i]);
        }
    }
    cout << f[m] << endl;
    
    return 0;
}
~~~



#### 分组背包

**题目描述**

有 N 组物品和一个容量是 V 的背包。

每组物品有若干个，同一组内的物品最多只能选一个。
每件物品的体积是 vij，价值是 wij，其中 i 是组号，j 是组内编号。

求解将哪些物品装入背包，可使物品总体积不超过背包容量，且总价值最大。

输出最大价值。

**输入格式**

第一行有两个整数 N，V，用空格隔开，分别表示物品组数和背包容量。

接下来有 N 组数据：

- 每组数据第一行有一个整数 Si，表示第 i 个物品组的物品数量；
- 每组数据接下来有 Si 行，每行有两个整数 vij,wij，用空格隔开，分别表示第 i 个物品组的第 j 个物品的体积和价值；

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

0<N,V≤100
0<Si≤100
0<vij,wij≤100

**输入样例**

```
3 521 22 413 414 5
```

**输出样例**

```
8
```



**代码**

~~~cpp
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 110;
int v[N][N], w[N][N], s[N];//s[i]用于记录每组的数
int f[N];
int n,m;

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>s[i];
        for(int j=0;j<s[i];j++){
            cin>>v[i][j]>>w[i][j];  //读入
        }
    }
    for(int i=1;i<=n;i++){//分组背包中模仿01背包求解
        for(int j=m;j>=0;j--){
            for(int k=0;k<s[i];k++){
                if(j>=v[i][k])     f[j]=max(f[j],f[j-v[i][k]]+w[i][k]);  
            }
        }
    }
    cout << f[m] << endl;
    return 0;
}
~~~



## 常用dp类型

### 线性dp

何谓线性dp？

答：递推方程有一个线性关系

#### 数字三角形

**题目描述**

给定一个如下图所示的数字三角形，从顶部出发，在每一结点可以选择移动至其左下方的结点或移动至其右下方的结点，一直走到底层，要求找出一条路径，使路径上的数字的和最大。

```
        7      3   8    8   1   0  2   7   4   44   5   2   6   5
```

**输入格式**

第一行包含整数 n，表示数字三角形的层数。

接下来 n 行，每行包含若干整数，其中第 i 行表示数字三角形第 i 层包含的整数。

**输出格式**

输出一个整数，表示最大的路径数字和。

**数据范围**

1≤n≤500,
−10000≤三角形中的整数≤10000

**输入样例**

```
573 88 1 0 2 7 4 44 5 2 6 5
```

**输出样例**

```
30
```



**分析**

~~~
状态表示f(i,j):所有从起点走到(i,j)的路径
			属性:Max
状态计算:f(i,j) -- [来自左上|来自右上]
			左上:f(i-1,j-1)+a(i,j)
			右上:f(i-1,j)+a(i,j)
状态转移方程f(i,j) = max(f(i-1,j-1),f(i-1,j))+a(i,j)
~~~



**核心代码**

~~~cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510, INF = 0x3f3f3f;
int n;
int v[N][N], f[N][N];

int main()
{
    cin >> n;
    
    for(int i = 1; i <= n; i ++)
        for(int j = 1; j <= i; j ++)
            cin >> v[i][j];
    
    for(int i = 0; i <= n + 1; i ++)//初始化的时候需要多初始化一些
        for(int j = 0; j <= n + 1; j ++)
            f[i][j] = -INF;
    
    f[1][1] = v[1][1];//第一行第一列的值 = v[1][1]
    for(int i = 2; i <= n; i ++)//从第二行进行递推
        for(int j = 1; j <= i; j ++)
            f[i][j] = max(f[i - 1][j - 1], f[i - 1][j]) + v[i][j];
    
    int res = -INF;
    for(int i = 1; i <= n; i ++)
        res = max(res, f[n][i]);
    
    cout << res << endl;
    return 0;
}
~~~



#### 最长上升子序列

**题目描述**

给定一个长度为 N 的数列，求数值严格单调递增的子序列的长度最长是多少。

**输入格式**

第一行包含整数 N。

第二行包含 N 个整数，表示完整序列。

**输出格式**

输出一个整数，表示最大长度。

**数据范围**

1≤N≤1000，
−109≤数列中的数≤109

**输入样例**

```
73 1 2 1 8 5 6
```

**输出样例**

```
4
```

**分析**

~~~
状态表示f[i]:所有以第i个数结尾的上升子序列集合
		属性: Max
状态计算f[i] -- [0|1|2|3|……|i-1]
		aj < ai: f[j]+1
状态转移f[i] = max(f[j]+1),j = 0,1,2,……,i-1
~~~

**核心代码**O(N^2)

~~~cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, INF = 0x3f3f3f3f;
int n;
int a[N],f[N];

int main()
{
    cin >> n;
    for(int i = 1; i <= n; i ++) cin >> a[i];
    
    for(int i = 1; i <= n; i ++){
        f[i] = 1;
        for(int j = 0; j <= i - 1; j ++)
            if(a[i] > a[j]){
                 f[i] = max(f[i], f[j] + 1);   
            }
    }
    
    int res = -INF;
    for(int i = 1; i <= n; i ++)
        res = max(res, f[i]);
        
    cout << res << endl;
    return 0;
}
~~~



**优化**

**思路**

~~~
将所有长度的最长上升子序列的最小值存在下，
猜想：随着最长上升子序列长度的增加，数值应该严格单调递增
增加一个数到数列中，找到一个数使之前一个数≤ai＜后一个数,接着用ai覆盖右边的数。
~~~



**代码**

~~~cpp
#include <iostream>

using namespace std;

const int N = 100010;
int n;
int a[N], q[N];

int main()
{
    cin >> n;
    
    for(int i = 0; i < n; i ++) cin >> a[i];
    
    int len = 0;
    q[0] = -2e9;
    for(int i = 0; i < n; i ++){//利用二分
        int l = 0, r = len;
        while(l < r){
            int mid = l + r + 1 >> 1;//向上取整
            if(q[mid] < a[i]) l = mid;
            else r = mid - 1;
        }
        len = max(len, r + 1);
        q[r + 1] = a[i];
    }
    cout << len << endl;
    return 0;
}
~~~



#### 最长公共子序列

**题目描述**

给定两个长度分别为 N 和 M 的字符串 A 和 B，求既是 A 的子序列又是 B 的子序列的字符串长度最长是多少。

**输入格式**

第一行包含两个整数 N 和 M。

第二行包含一个长度为 N 的字符串，表示字符串 A。

第三行包含一个长度为 M 的字符串，表示字符串 B。

字符串均由小写字母构成。

**输出格式**

输出一个整数，表示最大长度。

**数据范围**

1≤N,M≤1000

**输出样例**

```
4 5acbdabedc
```

**输出样例**

```
3
```



**题目分析**

~~~
状态表示f(i,j):所有在第一个序列的前i个字母中出现，且在第二个序列的前j个字母中出现的子序列
		属性:Max 
状态计算f(i,j) -- [00|01|10|11]
		00:不选ai、bj  --  f[i-1,j-1]
		01:不选ai、选bj  ∈  f[i-1,j]
		10:选ai、不选bj  ∈  f[i,j-1]
		11:选ai、bj  --  f[i-1,j-1] + 1
其中f[i-1,j-1]  ∈  [ f[i-1,j] | f[i,j-1] ]
~~~

**代码**

~~~cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;
char a[N], b[N];
int f[N][N];
int n, m;

int main()
{
    cin >> n >> m;
    scanf("%s%s",a + 1, b + 1);
    
    for(int i = 1; i <= n; i ++)
        for(int j = 0; j <= m; j ++){
            f[i][j] = max(f[i - 1][j], f[i][j - 1]);
            f[i][j] = max(f[i][j], f[i - 1][j - 1] + (a[i] == b[j]));
        }
    
    cout << f[n][m] << endl;
    return 0;
}
~~~



#### 最短编辑距离

**题目描述**

给定两个字符串 A 和 B，现在要将 A 经过若干操作变为 ，可进行的操作有：

1. 删除–将字符串 A 中的某个字符删除。
2. 插入–在字符串 A 的某个位置插入某个字符。
3. 替换–将字符串 A 中的某个字符替换为另一个字符。

现在请你求出，将 A 变为 B 至少需要进行多少次操作。

**输入格式**

第一行包含整数 n，表示字符串 A 的长度。

第二行包含一个长度为 n 的字符串 A。

第三行包含整数 m，表示字符串 B 的长度。

第四行包含一个长度为 m 的字符串 B。

字符串中均只包含大写字母。

**输出格式**

输出一个整数，表示最少操作次数。

**数据范围**

1≤n,m≤1000

**输入样例**

```
10 AGTCTGACGC11 AGTAAGTAGGC
```

**输出样例**

```
4
```



**分析**

~~~
状态表示f(i,j):将所有a[1~i]变成b[1~j]的操作方式
		属性:Min
状态计算f(i,j)  --  [删|增|改]
		删:a(i-1)与bj匹配  --  f(i-1,j)+1
		增:ai与b(j-1)匹配  --  f(i,j-1)+1
		改:i-1与j-1匹配 ai变成bj? --  f(i-1,j-1)+1/0
~~~



**代码**

~~~cpp
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1010;
int n, m;
char a[N], b[N];
int f[N][N];

int main(){
    scanf("%d%s", &n, a + 1);
    scanf("%d%s", &m, b + 1);
    
    for(int i = 0; i <= m; i ++) f[0][i] = i;//a[0]匹配b[i],增加a的长度
    for(int i = 0; i <= n; i ++) f[i][0] = i;//把a的前i个字母与b0匹配，删除a的字母长度
    
    for(int i = 1; i <= n; i ++){
        for(int j = 1; j <= m; j ++){
            f[i][j] = min(f[i - 1][j] + 1, f[i][j - 1] + 1);
            f[i][j] = min(f[i][j], f[i - 1][j - 1] + (a[i] != b[j]));
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
~~~



#### 编辑距离

**题目描述**

给定 n 个长度不超过 10 的字符串以及 m 次询问，每次询问给出一个字符串和一个操作次数上限。

对于每次询问，请你求出给定的 n 个字符串中有多少个字符串可以在上限操作次数内经过操作变成询问给出的字符串。

每个对字符串进行的单个字符的插入、删除或替换算作一次操作。

**输入格式**

第一行包含两个整数 n 和 m。

接下来 n 行，每行包含一个字符串，表示给定的字符串。

再接下来 m 行，每行包含一个字符串和一个整数，表示一次询问。

字符串中只包含小写字母，且长度均不超过 10。

**输出格式**

输出共 m 行，每行输出一个整数作为结果，表示一次询问中满足条件的字符串个数。

**数据范围**

1≤n,m≤1000

**输入样例**

```
3 2abcacdbcdab 1acbd 2
```

**输出样例**

```
13
```



**分析**

这题就是对最短编辑距离的应用



**代码**

~~~cpp
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int M = 1010;
int n, m;
char str[M][M];
int f[M][M];

int edit_distance(char a[],char b[])
{
    int la = strlen(a + 1), lb = strlen(b + 1);
    
    for(int i = 0; i <= lb; i ++) f[0][i] = i;
    for(int i = 0; i <= la; i ++) f[i][0] = i;
    
    for(int i = 1; i <= la; i ++){
        for(int j = 1; j <= lb; j ++){
            f[i][j] = min(f[i - 1][j] + 1, f[i][j - 1] + 1);
            f[i][j] = min(f[i][j], f[i - 1][j - 1] + (a[i] != b[j]));
        }
    }
    return f[la][lb];
}

int main()
{
    scanf("%d%d", &n, &m);
    
    for(int i = 0; i < n; i ++) scanf("%s", str[i] + 1);
    
    while(m --){
        char s[M];
        int limit;
        scanf("%s%d", s + 1, &limit);
        int res = 0;

        for(int i = 0; i < n; i ++)
            if(edit_distance(str[i], s) <= limit)
                res ++;
        
        printf("%d\n", res);
    }
    
    return 0;
}
~~~



### 区间dp

#### 石子合并

**题目描述**

设有 N 堆石子排成一排，其编号为 1，2，3，…，N。

每堆石子有一定的质量，可以用一个整数来描述，现在要将这 N 堆石子合并成为一堆。

每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆石子相邻的石子将和新堆相邻，合并时由于选择的顺序不同，合并的总代价也不相同。

例如有 4 堆石子分别为 `1 3 5 2`， 我们可以先合并 1、2 堆，代价为 4，得到 `4 5 2`， 又合并 1，2 堆，代价为 9，得到 `9 2` ，再合并得到 11，总代价为 4+9+11=24；

如果第二步是先合并 2，3 堆，则代价为 7，得到 `4 7`，最后一次合并代价为 11，总代价为 4+7+11=22。

问题是：找出一种合理的方法，使总的代价最小，输出最小代价。

**输入格式**

第一行一个数 N 表示石子的堆数 N。

第二行 N 个数，表示每堆石子的质量(均不超过 1000)。

**输出格式**

输出一个整数，表示最小代价。

**数据范围**

1≤N≤300

**输入样例**

```
41 3 5 2
```

**输出样例**

```
22
```



**题目分析**

~~~
状态表示f(i,j):所有将第i堆石子到第j堆石子合并成一堆石子的合并方式
			属性:Min
状态计算f(i,j) --  [1|2|3|……|k-2|k-1]

一堆石子   i———————————j
可以分成   [i,k],[k+1,j]
f[i,j] = min{f[i,k] + f[k+1,j] + s[j] - s[i-1]}k:[l,j-1]
~~~



**代码**

~~~cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 310;
int n;
int s[N];
int f[N][N];

int main(){
    cin >> n;

    for (int i = 1; i <= n; i ++) {
        cin >> s[i];
        s[i] += s[i - 1];
    }

    // 区间 DP 枚举套路：长度+左端点 
    for (int len = 1; len < n; len ++) { // len表示i和j堆下标的差值
        for (int i = 1; i + len <= n; i ++) {
            int j = i + len; // 自动得到右端点
            f[i][j] = 1e8;
            for (int k = i; k <= j - 1; k ++) { // 必须满足k + 1 <= j
                f[i][j] = min(f[i][j], f[i][k] + f[k + 1][j] + s[j] - s[i - 1]);
            }
        }
    }

    cout << f[1][n] << endl;

    return 0;
}
~~~



### 计数dp

#### 整数划分

**题目描述**

一个正整数 n 可以表示成若干个正整数之和，形如：n=n1+n2+…+nk，其中 n1≥n2≥…≥nk,k≥1。

我们将这样的一种表示称为正整数 n 的一种划分。

现在给定一个正整数 n，请你求出 n 共有多少种不同的划分方法。

**输入格式**

共一行，包含一个整数 n。

**输出格式**

共一行，包含一个整数，表示总划分数量。

由于答案可能很大，输出结果请对 109+7 取模。

**数据范围**

1≤n**≤**1000

**输入样例:**

```
5
```

**输出样例：**

```
7
```



**法一分析**

~~~
状态表示f(i,j):从1~i中选,体积恰好为j
			属性:数量
状态计算f(i,j)  --  [0|1|2|……|s]
			0  --  f(i-1,j)
			1  --  f(i-1,j-i)
			s  --  f(i-1,j-si)
可以从完全背包问题角度来思考
即将每个背包容量变为i
~~~



**代码**

~~~cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, mod = 1e9 + 7;
int n;
int f[N];

int main()
{
    cin >> n;
    f[0] = 1;
    for(int i = 1;i <= n; i ++){
        for(int j = i; j <= n; j++){
            f[j] = (f[j] + f[j-i]) % mod;
        }
    }
    cout << f[n] << endl;
    
    return 0;
}
~~~



**法二分析**

~~~
状态表示f(i,j):所有总和是i并且恰好表示成j个数的和的方案
			属性:数量
状态计算f(i,j)  --  [最小值1|最小值>1]
			最小值=1:f(i-1,j-1)
			最小值>1:f(i-j,j)
状态转移方程f(i,j) = f(i-1,j-1)+f(i-j,j)
~~~



**代码**

~~~cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, mod = 1e9 + 7;
int n;
int f[N][N];

int main()
{
    cin >> n;
    f[0][0] = 1;
    for(int i = 1; i <= n; i ++){
        for(int j = 1; j <= i; j ++){
            f[i][j] = (f[i - 1][j - 1] + f[i - j][j]) % mod;
        }
    }
    
    int res = 0;
    for(int i = 1; i <= n; i ++){
        res = (res + f[n][i]) % mod;
    }
    cout << res << endl;
}
~~~



### 树形dp

#### 没有上司的舞会

**题目描述**

Ural 大学有 N 名职员，编号为 1∼N。

他们的关系就像一棵以校长为根的树，父节点就是子节点的直接上司。

每个职员有一个快乐指数，用整数 Hi 给出，其中 1≤i≤N。

现在要召开一场周年庆宴会，不过，没有职员愿意和直接上司一起参会。

在满足这个条件的前提下，主办方希望邀请一部分职员参会，使得所有参会职员的快乐指数总和最大，求这个最大值。

**输入格式**

第一行一个整数 N。

接下来 N 行，第 ii 行表示 ii 号职员的快乐指数 Hi。

接下来 N−1 行，每行输入一对整数 L,K，表示 K 是 L 的直接上司。

**输出格式**

输出最大的快乐指数。

**数据范围**

1≤N≤6000,
−128≤Hi≤127

**输入样例：**

```
711111111 32 36 47 44 53 5
```

**输出样例：**

```
5
```



**分析**

~~~
状态表示f[u,0]:所有以u为根的子树中选，并且不选u这个点的方案
       f[u,1]:所有以u为根的子树中选，并且选u的方案
     属性:Max
状态计算  f[u,0]  --  ∑max(f(si,0),f(si,1))
		 f[u,1]  --  ∑f(si,0)
~~~



**代码**

~~~cpp
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 6010;
int n;
int happy[N];
int h[N], e[N], ne[N], idx;
int f[N][2];
bool has_father[N];

void add(int a, int b){
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx ++;
}

void dfs(int u){
    f[u][1] = happy[u];
    
    for(int i = h[u]; i != -1; i = ne[i]){
        int j = e[i];
        dfs(j);
        f[u][0] += max(f[j][0], f[j][1]);
        f[u][1] += f[j][0];
    }
}

int main()
{
    cin >> n;
    
    for(int i = 1; i <= n; i ++) cin >> happy[i];
    
    memset(h, -1, sizeof h);
    
    for(int i = 0; i < n - 1; i ++){
        int a, b;
        scanf("%d%d", &a, &b);
        has_father[a] = true;
        add(b, a);
    }
    
    int root = 1;
    
    while(has_father[root]) root ++;
    
    dfs(root);
    
    printf("%d\n",max(f[root][0],f[root][1]));
    return 0;
}
~~~



### 记忆化搜索

#### 滑雪

**题目描述**

给定一个 R 行 C 列的矩阵，表示一个矩形网格滑雪场。

矩阵中第 i 行第 j 列的点表示滑雪场的第 i 行第 j 列区域的高度。

一个人从滑雪场中的某个区域内出发，每次可以向上下左右任意一个方向滑动一个单位距离。

当然，一个人能够滑动到某相邻区域的前提是该区域的高度低于自己目前所在区域的高度。

下面给出一个矩阵作为例子：

```
 1  2  3  4 516 17 18 19 615 24 25 20 714 23 22 21 813 12 11 10 9
```

在给定矩阵中，一条可行的滑行轨迹为 24−17−2−1。

在给定矩阵中，最长的滑行轨迹为 25−24−23−…−3−2−1，沿途共经过 25 个区域。

现在给定你一个二维矩阵表示滑雪场各区域的高度，请你找出在该滑雪场中能够完成的最长滑雪轨迹，并输出其长度(可经过最大区域数)。

**输入格式**

第一行包含两个整数 R 和 C。

接下来 R 行，每行包含 C 个整数，表示完整的二维矩阵。

**输出格式**

输出一个整数，表示可完成的最长滑雪长度。

**数据范围**

1≤R,C≤300,
0≤矩阵中整数≤10000

**输入样例：**

```
5 51 2 3 4 516 17 18 19 615 24 25 20 714 23 22 21 813 12 11 10 9
```

**输出样例：**

```
25
```



**题目分析**

~~~
状态表示f(i,j):所有从(i,j)开始滑的路径
			属性:Max
状态计算f(i,j)  --  max{存在[上|右|左|下]}
			上:f(i-1,j)+1
			右:f(i,j+1)+1
			左:f(i,j-1)+1
			下:f(i+1,j)+1
~~~



**代码**

~~~cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 310;

int n, m;
int h[N][N];
int f[N][N];
int next[4][2] = {{0,1},{1,0},{0,-1},{-1,0}};

int dp(int x, int y){
    int &v = f[x][y];
    if(v != -1) return v;
    
    v = 1;
    for(int k = 0; k < 4; k ++){
        int tx = x + next[k][0], ty = y + next[k][1];
        if(tx >= 1 && tx <= n && ty >= 1 && ty <= m && h[tx][ty] < h[x][y]){
            v = max(v, dp(tx, ty) + 1);
        }
    }
    return v;
}

int main()
{
    scanf("%d%d", &n, &m);
    
    for(int i = 1; i <= n; i ++){
        for(int j = 1; j <= m; j ++){
            cin >> h[i][j];
        }
    }
    memset(f, -1, sizeof f);
    
    int res = 0;
    for(int i = 1; i <= n; i ++){
        for(int j = 1; j <= m; j ++){
            res = max(res, dp(i, j));
        }
    }
    cout << res << endl;
    return 0;
}
~~~



# 总结

### dp优化

动态规划的优化一般是将原先的式子做等价变形，因此需要先写出原先的式子



### dp通解

~~~
dp: 状态表示f(i,j)//考虑维度
		集合?
		属性Max,Min,数量?
	状态计算--集合划分，看最后一步
				不重:当属性为数量不能够重复
				不漏
~~~

