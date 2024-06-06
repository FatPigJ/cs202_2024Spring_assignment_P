# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024



**说明：**

1）数算课程的先修课是计概，由于计概学习中可能使用了不同的编程语言，而数算课程要求Python语言，因此第一周作业练习Python编程。如果有同学坚持使用C/C++，也可以，但是建议也要会Python语言。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：PyCharm



## 1. 题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/



思路：维护一个Tn列表，然后按定义计算



##### 代码

```python
T=[0,1,1]
n=int(input())
for i in range(3,n+1):
    T.append(T[-1]+T[-2]+T[-3])
print(T[n])
```



代码运行截





### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A



思路：正则表达式



##### 代码

```python
s = input()
import re
if re.match(r'.*h.*e.*l.*l.*o',s):
    print("YES")
else:
    print("NO")
```

代码运行截图 







### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A



思路：正则表达式



##### 代码

```python
s = input()
import re
s = re.sub(r'[AEIOUYaeiouy]','',s)
for c in s:
    print('.'+c.lower(), end='')
```



代码运行截图 







### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/



思路：欧拉筛



##### 代码

```python
S = int(input())

primelist = []
filter = [1 for i in range(S+1)]
for i in range(2, S):
    if filter[i]:
        primelist.append(i)
        for p in primelist:
            if i*p > S:
                break
            filter[i*p] = 0
    else:
        for p in primelist:
            if i*p > S:
                break
            filter[i*p] = 0
            if i%p == 0:
                break
primes = set(primelist)

for p in primelist:
    if S-p in primes:
        print(p, S-p)
        break
```



代码运行截图 






### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/



思路：运用字符串的分割，排序方法



##### 代码

```python
s = input().split('+')
s = [i.split('n^') for i in s]
s.sort(key=lambda x:int(x[1]))
s.reverse()
for i in s:
    if i[0] != '0':
        print('n^'+i[1])
        break
```



代码运行截图





### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/



思路：使用defaultdict



##### 代码

```python
from collections import defaultdict
choice_votes = defaultdict(int)
votes_choice = defaultdict(list)

for c in map(int,input().split()):
    choice_votes[c] += 1

for c, v in choice_votes.items():
    votes_choice[v].append(c)

print(*sorted(votes_choice[max(votes_choice.keys())]))
```



代码运行截图





## 2. 学习总结和收获

好久没动过python，语法很多都不熟悉了





