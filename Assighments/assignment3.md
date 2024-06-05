# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 10

Python编程环境：Pycharm



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：dfs



##### 代码

```python
k = int(input())
missles = list(map(int, input().split()))

def count(i, m):
    if i < k-1:
        if missles[i] <= m:
            return max(count(i+1, m), count(i+1, missles[i])+1)
        else:
            return count(i+1, m)
    else:
        if missles[i] <= m:
            return 1
        else:
            return 0

print(count(0, float('inf')))
```



代码运行截图 





**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：recursion



##### 代码

```python
n, a, b, c = input().split()
n = int(n)
poles = [a, b, c]
def hanoi(m, fr, to):
    if m == 1:
        return ["1:{}->{}".format(fr, to)]
    else:
        for p in poles:
            if p != fr and p != to:
                mid = p
                break
        return hanoi(m-1, fr, mid) + ["{}:{}->{}".format(m, fr, to)] + hanoi(m-1, mid, to)

L = hanoi(n, a, c)
print(*L, sep='\n')
```



代码运行截图 







**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：queue



##### 代码

```python
from collections import deque

while 1:
    n, p, m = map(int, input().split())
    if n == 0:
        break

    queue = deque(list(range(p, n+1))+list(range(1, p)))
    ans = []
    while len(queue) > 1:
        for i in range(m-1):
            queue.append(queue.popleft())
        ans.append(queue.popleft())

    ans.append(queue.popleft())
    print(*ans, sep=',')
```



代码运行截图







**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：greedy



##### 代码

```python
n = int(input())
seq = list(enumerate(map(int, input().split())))
seq.sort(key=lambda x:x[1])
order = [i[0]+1 for i in seq]
aver = sum([seq[i-1][1] * (n - i) for i in range(1, n+1)]) / n
print(*order)
print('%.2f'%aver)
```



代码运行截图 







**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：严格按照题意



##### 代码

```python
n=int(input())
pairs = [i[1:-1] for i in input().split()]
distance = [sum(map(int,i.split(','))) for i in pairs]
price=list(map(int,input().split()))
ratio=[distance[i]/price[i] for i in range(n)]
ratios=sorted(ratio)
prices=sorted(price)

if n%2==0:
    ratmed=(ratios[n//2]+ratios[n//2-1])/2
    primed=(prices[n//2]+prices[n//2-1])/2
else:
    ratmed=ratios[(n-1)//2]
    primed = prices[(n - 1) //2]

ans=0
for i in range(n):
    if ratio[i]>ratmed and price[i]<primed:
        ans+=1
print(ans)
```



代码运行截图







**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：使用参数量/M作为排序依据，使用defaultdict包存储数据，使用x % 1 == 0.0作为判断是否为整数的依据



##### 代码

```python
from collections import defaultdict

def unit_convert(string):
    in_m = float(string[:-1]) if string[-1] == 'M' else 1000 * float(string[:-1])
    if in_m < 1000:
        return [str(int(in_m) if in_m % 1 == 0.0 else in_m) + 'M', in_m]
    else:
        return [str(int(in_m/1000) if in_m/1000 % 1 == 0.0 else in_m/1000) + 'B', in_m]

n = int(input())
dic = defaultdict(list)
for _ in range(n):
    name, quan = input().split('-')
    dic[name].append(unit_convert(quan))

for name in sorted(dic.keys()):
    quans = dic[name]
    quans.sort(key=lambda x: x[-1])
    print(name + ': ', end='')
    print(*[i[0] for i in quans], sep=', ')
```



代码运行截图







## 2. 学习总结和收获

数据量小的题不用硬想dp等时间复杂度低的算法，可以直接遍历找到答案。





