# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：Pycharm



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/practice/27653/



思路：

调用math中的gcd

##### 代码

```python
import math
class frac(object):
    def __init__(self, num, den):
        d = math.gcd(num, den)
        self.num = num // d
        self.den = den // d

    def __add__(self, other):
        return frac(self.num*other.den + other.num*self.den, self.den*other.den)

    def __str__(self):
        return str(self.num)+'/'+str(self.den)

a, b, c, d = map(int, input().split())

print(frac(a, b) + frac(c, d))
```



代码运行截图







### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：greedy



##### 代码

```python
ip1=input().split()
n=int(ip1[0]);w=int(ip1[1])
candies=[]
for i in range(n):
    candies.append(list(map(int,input().split())))
candies.sort(key=lambda x:x[0]/x[1],reverse=1)

ttl=0;w1=0
for i in candies:
    if w1+i[1]>=w:
        ttl+=i[0]*((w-w1)/i[1])
        break
    else:
        ttl+=i[0]
        w1+=i[1]
print('%.1f'%ttl)
```



代码运行截图







### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：使用greedy，defaultdict



##### 代码

```python
from collections import defaultdict
case_num = int(input())
for test_case in range(case_num):
    n, m, b = map(int, input().split())
    skills = defaultdict(list)
    for skill_no in range(n):
        t, x = map(int, input().split())
        skills[t].append(x)

    for t, tskills in sorted(skills.items()):
        tskills.sort()
        tskills.reverse()
        b -= sum(tskills[:m])

        if b <= 0:
            print(t)
            break

    if b > 0:
        print("alive")
```



代码运行截图 







### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：Euler筛



##### 代码

```python
import math
 
n = int(input())
arr = list(map(int, input().split()))
M = math.ceil(math.sqrt(max(arr)))
 
primelist = []
fltr = [1 for i in range(M+1)]
for i in range(2, M+1):
    if fltr[i]:
        primelist.append(i)
        for p in primelist:
            if i*p > M:
                break
            fltr[i*p] = 0
    else:
        for p in primelist:
            if i*p > M:
                break
            fltr[i*p] = 0
            if i%p == 0:
                break
tprimes = set(map(lambda x: x**2, primelist))
 
for i in arr:
    if i in tprimes:
        print('YES')
    else:
        print("NO")
```



代码运行截图 







### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：先判断整个array是否满足要求，若否则首尾双指针看开头结尾哪个数不能被x整除



##### 代码

```python
t = int(input())
for test_case in range(t):
    n, x = map(int, input().split())
    array = list(map(int, input().split()))

    if sum(array) % x != 0:
        print(n)
        continue

    flag = 1
    for i in range(0, (n+1)//2):
        j = -1 - i
        if array[i] % x != 0 or array[j] % x != 0:
            print(n+j)
            flag = 0
            break

    if flag:
        print(-1)
```



代码运行截图 







### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：Euler筛



##### 代码

```python
from math import sqrt, pow, ceil
m, n = map(int, input().split())
scores = []
for stu in range(m):
    scores.append(list(map(int, input().split())))
M = max(map(max, scores))
M = ceil(sqrt(M))

primelist = []
fltr = [1 for i in range(M+1)]
for i in range(2, M+1):
    if fltr[i]:
        primelist.append(i)
        for p in primelist:
            if i*p > M:
                break
            fltr[i*p] = 0
    else:
        for p in primelist:
            if i*p > M:
                break
            fltr[i*p] = 0
            if i%p == 0:
                break
tprimes = set(map(lambda x:pow(x, 2), primelist))

for score in scores:
    valid_score = list(filter(lambda x: x in tprimes, score))
    if valid_score:
        print("%.2f"%(sum(valid_score) / len(score)))
    else:
        print(0)
```



代码运行截图 







## 2. 学习总结和收获

有好多上学期做过的题，但是好多忘掉了





