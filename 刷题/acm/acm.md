# 算法

## 排序与高精度

### 排序

常见的排序算法：快速排序，归并排序，堆排序，桶排序。

### 高精度加减法

```c
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

bool cmp(vector<int> &A, vector<int> &B)
{
    if (A.size() != B.size())
        return A.size() > B.size();
    for (int i = A.size() - 1; i >= 0; i--)
    {
        if (A[i] != B[i])
            return A[i] > B[i];
    }
    return true;
}

vector<int> add(vector<int> a, vector<int> b)
{
    vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size() || i < b.size(); i++)
    {
        if (i < a.size())
            t += a[i];
        if (i < b.size())
            t += b[i];
        c.push_back(t % 10);
        t /= 10;
    }
    if (t)
        c.push_back(1);
    return c;
}

vector<int> sub(vector<int> a, vector<int> b)
{
    vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size(); i++)
    {
        t = a[i] - t;
        if (i < b.size())
            t -= b[i];
        c.push_back((t + 10) % 10);
        if (t < 0)
            t = 1;
        else
            t = 0;
    }
    while (c.size() > 1 && c.back() == 0)
        c.pop_back();

    return c;
}

int main()
{
    string a, b;
    cin >> a >> b;

    vector<int> a1, b1;
    for (int i = a.size() - 1; i >= 0; i--)
        a1.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i--)
        b1.push_back(b[i] - '0');

    auto add_c = add(a1, b1);
    cout << a << " add " << b << " = ";
    for (int i = add_c.size() - 1; i >= 0; i--)
        cout << add_c[i];
    cout << endl;

    vector<int> sub_c;
    if (cmp(a1, b1))
        sub_c = sub(a1, b1);
    else
        sub_c = sub(b1, a1);

    cout << a << " sub " << b << " = ";
    for (int i = sub_c.size() - 1; i >= 0; i--)
        cout << sub_c[i];
    cout << endl;

    return 0;
}
```

### 高精度乘除法

```c
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

vector<int> mul(vector<int> a, int b)
{
    vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size() || t; i++)
    {
        if (i < a.size())
            t += a[i] * b;
        c.push_back(t % 10);
        t /= 10;
    }
    return c;
}

vector<int> div(vector<int> a, int b)
{
    vector<int> c;
    int t = 0;
    for (int i = a.size() - 1; i >= 0; i--)
    {
        t = t * 10 + a[i];
        c.push_back(t / b);
        t %= b;
    }
    reverse(c.begin(), c.end());
    while (c.back() == 0)
        c.pop_back();
    return c;
}

int main()
{
    string a;
    int b;
    cin >> a >> b;

    vector<int> a1;
    for (int i = a.size() - 1; i >= 0; i--)
        a1.push_back(a[i] - '0');

    auto mul_c = mul(a1, b);
    cout << a << " mul " << b << " = ";
    for (int i = mul_c.size() - 1; i >= 0; i--)
        cout << mul_c[i];
    cout << endl;

    auto div_c = div(a1, b);
    cout << a << " div " << b << " = ";
    for (int i = div_c.size() - 1; i >= 0; i--)
        cout << div_c[i];
    cout << endl;
    return 0;
}
```

### 题单

#### [车厢重组(冒泡排序)](https://www.luogu.com.cn/problem/P1116)

==题目描述==

在一个旧式的火车站旁边有一座桥，其桥面可以绕河中心的桥墩水平旋转。一个车站的职工发现桥的长度最多能容纳两节车厢，如果将桥旋转 $180$ 度，则可以把相邻两节车厢的位置交换，用这种方法可以重新排列车厢的顺序。于是他就负责用这座桥将进站的车厢按车厢号从小到大排列。他退休后，火车站决定将这一工作自动化，其中一项重要的工作是编一个程序，输入初始的车厢顺序，计算最少用多少步就能将车厢排序。

==输入格式==

共两行。  

第一行是车厢总数 $N( \le 10000)$。

第二行是 $N$ 个不同的数表示初始的车厢顺序。

==输出格式==

一个整数，最少的旋转次数。

==样例输入==

```
4
4 3 2 1
```

==样例输出==

```
6
```

==代码实现==

```c
#include <iostream>
using namespace std;

const int N = 1e5 + 5;
int a[N];

int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i++)
        cin >> a[i];

    int ans = 0;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n - i - 1; j++)
            if (a[j] > a[j + 1])
            {
                int temp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = temp;
                ans++;
            }

    cout << ans << endl;
    return 0;
}
```

#### [数楼梯](https://www.luogu.com.cn/problem/P1255)(高精)

==题目描述==

楼梯有 $N$ 阶，上楼可以一步上一阶，也可以一步上二阶。

编一个程序，计算共有多少种不同的走法。

==输入格式==

一个数字，楼梯数。

==输出格式==

输出走的方式总数。

==样例输入== 

```
4
```

==样例输出==

```
5
```

==提示==

- 对于 $60\%$ 的数据，$N \leq 50$；   
- 对于 $100\%$ 的数据，$1 \le N \leq 5000$。

==笔记==

我们可以通过推理来找出。

第3位可以由第1阶走两步或第2阶走一步得出。

第4位可以由第2阶走两步或第3阶走一步得出。

第5位可以由第3阶走两步或第4阶走一步得出。

......

最后可以得出：

a[i]=a[i-1]+a[i-2];

==代码实现==

```c
#include <iostream>
#include <string>
#include <vector>
using namespace std;

string add(string a, string b)
{
    string c;
    vector<int> A, B, C;
    for (int i = a.size() - 1; i >= 0; i--)
        A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i--)
        B.push_back(b[i] - '0');

    int t = 0;
    for (int i = 0; i < A.size() || i < B.size(); i++)
    {
        if (i < A.size())
            t += A[i];
        if (i < B.size())
            t += B[i];

        C.push_back(t % 10);
        t /= 10;
    }
    if (t)
        C.push_back(t);

    for (int i = 0; i < C.size(); i++)
    {
        c = char(C[i] + '0') + c;
    }
    return c;
}

int main()
{
    int n;
    cin >> n;

    string a = "1", b = "2";
    if (n == 1)
        cout << 1 << endl;
    else
    {

        for (int i = 3; i <= n; i++)
        {
            string temp = add(a, b);
            a = b;
            b = temp;
        }
        cout << b;
    }

    return 0;
}
```

#### [[NOIP2014 普及组] 珠心算测验(桶)](https://www.luogu.com.cn/problem/P2141)

题目描述

珠心算是一种通过在脑中模拟算盘变化来完成快速运算的一种计算技术。珠心算训练，既能够开发智力，又能够为日常生活带来很多便利，因而在很多学校得到普及。


某学校的珠心算老师采用一种快速考察珠心算加法能力的测验方法。他随机生成一个正整数集合，集合中的数各不相同，然后要求学生回答：其中有多少个数，恰好等于集合中另外两个（不同的）数之和？


最近老师出了一些测验题，请你帮忙求出答案。


(本题目为 2014NOIP 普及 T1)

==输入格式==

共两行，第一行包含一个整数 $n$，表示测试题中给出的正整数个数。


第二行有 $n$ 个正整数，每两个正整数之间用一个空格隔开，表示测试题中给出的正整数。

==输出格式==

一个整数，表示测验题答案。

==样例输入==

```
4
1 2 3 4
```

==样例输出==

```
2
```

==提示==

【样例说明】


由 $1+2=3,1+3=4$，故满足测试要求的答案为 $2$。  

注意，加数和被加数必须是集合中的两个不同的数。


【数据说明】

对于 $100\%$ 的数据，$3 \leq n \leq 100$，测验题给出的正整数大小不超过 $10,000$。

==代码实现==

```c
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

const int N = 1e5 + 5;
int t[N], g[N];

int main()
{
    int n;
    cin >> n;
    int a[105];

    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
        g[a[i]] = 1;
    }

    int maxn = 0;

    for (int i = 0; i < n - 1; i++)
        for (int j = i + 1; j < n; j++)
        {
            t[a[i] + a[j]]++;
            maxn = max(maxn, a[i] + a[j]);
        }

    int ans = 0;
    for (int i = 0; i <= maxn; i++)
        if (t[i] && g[i])
            ans++;

    cout << ans;
    return 0;
}
```

#### [[NOIP1998 普及组] 阶乘之和(高精乘加)](https://www.luogu.com.cn/problem/P1009)

==题目描述==

用高精度计算出 $S = 1! + 2! + 3! + \cdots + n!$（$n \le 50$）。

其中!表示阶乘，定义为 $n!=n\times (n-1)\times (n-2)\times \cdots \times 1$。例如，$5! = 5 \times 4 \times 3 \times 2 \times 1=120$。

==输入格式==

一个正整数 $n$。

==输出格式==

一个正整数 $S$，表示计算结果。

==样例输入==

```
3
```

==样例输出==

```
9
```

==提示==

**【数据范围】**

对于 $100 \%$ 的数据，$1 \le n \le 50$。

**【其他说明】**

==代码实现==

```c
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

void mul(string &a, int b)
{
    string c;
    vector<int> A, C;
    for (int i = a.size() - 1; i >= 0; i--)
        A.push_back(a[i] - '0');

    int t = 0;
    for (int i = 0; i < A.size() || t; i++)
    {
        if (i < A.size())
            t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }
    a.clear();
    for (int i = 0; i < C.size(); i++)
        a = char(C[i] + '0') + a;
}

string add(string a, string b)
{
    string c;
    vector<int> A, B, C;

    for (int i = a.size() - 1; i >= 0; i--)
        A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i--)
        B.push_back(b[i] - '0');

    int t = 0;
    for (int i = 0; i < a.size() || i < b.size(); i++)
    {
        if (i < a.size())
            t += A[i];
        if (i < b.size())
            t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    if (t)
        C.push_back(t);
    for (int i = 0; i < C.size(); i++)
        c = char(C[i] + '0') + c;
    return c;
}

int main()
{
    int n;
    cin >> n;

    string sum = "0";
    for (int i = 1; i <= n; i++)
    {
        string temp;
        int num = i;
        while (num)
        {
            temp = char((num % 10) + '0') + temp;
            num /= 10;
        }
        for (int j = i - 1; j > 0; j--)
        {
            mul(temp, j);
        }
        sum = add(temp, sum);
    }
    cout << sum << endl;
    return 0;
}
```

#### [高精度减法](https://www.luogu.com.cn/problem/P2142)

==题目描述==

高精度减法。

==输入格式==

两个整数 $a,b$（第二个可能比第一个大）。

==输出格式==

结果（是负数要输出负号）。

==样例输入==

```
2
1
```

==样例输出==

```
1
```

==提示==

- $20\%$ 数据 $a,b$ 在 long long 范围内；
- $100\%$ 数据 $0<a,b\le 10^{10086}$。

==代码实现==

```c
#include <iostream>
#include <string>
#include <vector>
using namespace std;

bool cmp(vector<int> a, vector<int> b)
{
    if (a.size() != b.size())
        return a.size() > b.size();
    else
    {
        for (int i = a.size() - 1; i >= 0; i--)
        {
            if (a[i] < b[i])
                return false;
        }
    }
    return true;
}

vector<int> sub(vector<int> A, vector<int> B)
{
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i++)
    {
        t += A[i];
        if (i < B.size())
            t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0)
            t = -1;
        else
            t = 0;
    }
    while (C.size() > 1 && C.back() == 0)
        C.pop_back();
    return C;
}

int main()
{
    string a, b;
    cin >> a >> b;

    vector<int> A, B;
    for (int i = a.size() - 1; i >= 0; i--)
        A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i--)
        B.push_back(b[i] - '0');

    vector<int> C;
    if (cmp(A, B))
    {
        C = sub(A, B);
    }
    else
    {
        C = sub(B, A);
        cout << "-";
        for (int i = C.size() - 1; i >= 0; i--)
            cout << C[i];
    }

    return 0;
}
```

## 前缀和和差分

### 前缀和

#### 定义

sum[i] = $\sum_{i = 0}^{n}a_j$

指定区间[L,R],区间和为Ans = sum[R] - sum[L-1]

#### 代码实现

给定n(1e7)个整数，Q(1e7)次询问，每次询问一个指定区间的区间和（区间内数的和)

```c
#include <iostream>
using namespace std;

const int N = 1e7 + 5;
int n, Q, a[N], sum[N];

int main()
{
    cin >> n;
    sum[0] = 0;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
        sum[i] = sum[i - 1] + a[i];
    }
    cin >> Q;
    while (Q--)
    {
        int L, R;
        cin >> L >> R;
        cout << sum[R] - sum[L - 1] << endl;
    }

    return 0;
}
```

### 前缀异或和

#### 定义

sum[i] = a1 $\bigoplus$ a2 $\bigoplus$ ... $\bigoplus$ ai

sum[L-1] = sum[R] , 则区间[L,R]的异或和为0 

#### 代码实现

给定n个整数，求异或和为0的区间数量

```c
#include <iostream>
#include <map>
#define ll long long
using namespace std;
const int maxn = 2e5 + 5;

std::map<ll, ll> mp;
ll a[maxn], b[maxn];

int main()
{
    int n;
    cin >> n;
    ll ans = 0;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
        b[i] = a[i] ^ b[i - 1];
        if (b[i] == 0)
            ans++;
        ans += mp[b[i]];
        mp[b[i]]++;
        cout << ans << endl;
    }
    cout << ans << endl;
    return 0;
}
```

### 二位前缀和

给定一个 n*n 的矩阵,求任意矩形内的数之和  

```c
#include <iostream>
using namespace std;

const int N = 1e3 + 5;
int n, Q, a[N][N], sum[N][N];

//(x1,y1)为矩阵左上角,(x2.y2)为矩阵右下角
int Query(int x1, int y1, int x2, int y2)
{
    return sum[x2][y2] - sum[x1 - 1][y2] - sum[x2][y1 - 1] + sum[x1 - 1][y1 - 1];
}

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
        {
            cin >> a[i][j];
            sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + a[i][j];
        }

    cin >> Q;
    while (Q--)
    {
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        cout << Query(x1, y1, x2, y2) << endl;
    }

    return 0;
}
```

### 差分

#### 定义

b~i~ = a~i~ - a~i-1~(i>1) or a~1~(i=1) 

b~i~ 的前缀和为原数组a~i~

#### 代码实现

给定区间\[L,R](length<1e7)加减的操作，求最终结果数组。  

```c
#include <iostream>
using namespace std;

const int N = 1e7 + 5;
int n, a[N], b[N], sum[N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
        b[i] = a[i] - a[i - 1];
    }

    int L, R, num;
    cin >> L >> R >> num;

    b[L] += num, b[R + 1] += num;
    for (int i = 1; i <= n; i++)
    {
        cout << a[i] << " ";
        sum[i] = sum[i - 1] + b[i];
    }
    cout << endl;
    for (int i = 1; i <= n; i++)
        cout << sum[i] << " ";

    return 0;
}
```

### 题目

#### CF1704D  Magical Array





