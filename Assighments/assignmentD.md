# Assignment #D: May月考

Updated 1654 GMT+8 May 8, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：PyCharm 



## 1. 题目

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：用list模拟



代码

```python
def read():
    return map(int, input().split())

L, M = read()
road = [1 for _ in range(L+1)]
for _ in range(M):
    start, end = read()
    for i in range(start, end+1):
        road[i] = 0
print(sum(road))
```



代码运行截图 ==（至少包含有"Accepted"）==







### 20449: 是否被5整除

http://cs101.openjudge.cn/practice/20449/



思路：

使用int函数转换

代码

```python
string = input()
print(*(int(int(string[:i + 1], 2) % 5 == 0) for i in range(len(string))), sep='')
```



代码运行截图







### 01258: Agri-Net

http://cs101.openjudge.cn/practice/01258/



思路：prim算法



代码

```python
from heapq import heappop, heappush, heapify

def prim(mat, size):
    vert_list = [float('inf') for _ in range(size)]
    visited = set()
    heap = [(0, 0)]
    heapify(heap)
    result = 0
    while heap:
        weight, vert = heappop(heap)
        if vert in visited:
            continue
        visited.add(vert)
        result += weight
        for nbr, weight in enumerate(mat[vert]):
            if nbr not in visited and nbr != vert and vert_list[nbr] > weight:
                vert_list[nbr] = weight
                heappush(heap, (weight, nbr))
    return result

while 1:
    try:
        N = int(input())
        mat = []
        for _ in range(N):
            mat.append(list(map(int, input().split())))
        print(prim(mat, N))
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==







### 27635: 判断无向图是否连通有无回路(同23163)

http://cs101.openjudge.cn/practice/27635/



思路：

使用两个dfs，第二个dfs当图不连通且还未找到回路时使用

代码

```python
def read():
    return map(int, input().split())

def dfs(prev, vert, graph, visited):
    global loop
    visited[vert] = True
    for nbr in graph[vert] - {prev}:
        if visited[nbr]:
            loop = True
        else:
            dfs(vert, nbr, graph, visited)

def dfs2(prev, vert, graph, visited):
    global loop
    visited[vert] = True
    for nbr in graph[vert] - {prev}:
        if visited[nbr]:
            loop = True
        else:
            dfs(vert, nbr, graph, visited)
        if loop:
            break

n, m = read()
graph = {vert: set() for vert in range(n)}
for _ in range(m):
    v1, v2 = read()
    graph[v1].add(v2)
    graph[v2].add(v1)
loop = False
dfs(None, 0, graph, visited := [False for _ in range(n)])
connected = False not in visited

if (not connected) and (not loop):
    for vert in range(n):
        if not visited[vert]:
            dfs2(None, vert, graph, visited)
            if loop:
                break

connected = 'yes' if connected else 'no'
loop = 'yes' if loop else 'no'
print(f"connected:{connected}")
print(f"loop:{loop}")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==









### 27947: 动态中位数

http://cs101.openjudge.cn/practice/27947/



思路：

使用两个堆

代码

```python
from heapq import heappop, heappush,heapify
T = int(input())
for _ in range(T):
    arr = list(map(int, input().split()))
    l = len(arr)
    result = [arr[0]]
    heapl, heaph = [], [arr[0]]
    heapify(heapl); heapify(heaph)
    for k in range(2, l, 2):
        if arr[k-1] < arr[k]:
            heappush(heapl, -arr[k-1])
            heappush(heaph, arr[k])
        else:
            heappush(heapl, -arr[k])
            heappush(heaph, arr[k-1])

        while -heapl[0] > heaph[0]:
            heappush(heaph, -heappop(heapl))
            heappush(heapl, -heappop(heaph))
        result.append(heaph[0])

    print((l+1) // 2)
    print(*result)
```



代码运行截图







### 28190: 奶牛排队

http://cs101.openjudge.cn/practice/28190/



思路：

参照题解，单调栈

代码

```python
N = int(input())
arr = [int(input()) for _ in range(N)]

gt = [-1 for _ in range(N)]
mono_stack = []
for cow in range(N):
    height = arr[cow]
    while mono_stack and mono_stack[-1][1] < height:
        mono_stack.pop()
    if mono_stack:
        gt[cow] = mono_stack[-1][0]
    mono_stack.append((cow, height))

lt = [N for _ in range(N)]
mono_stack = []
for cow in range(N-1, -1, -1):
    height = arr[cow]
    while mono_stack and mono_stack[-1][1] > height:
        mono_stack.pop()
    if mono_stack:
        lt[cow] = mono_stack[-1][0]
    mono_stack.append((cow, height))

ans = 0
for end in range(N):
    for start in range(gt[end]+1, end):
        if lt[start] > end:
            ans = max(ans, end - start + 1)
            break
print(ans)
```



代码运行截图







## 2. 学习总结和收获

忘记了栈顶索引是-1

最后一题时间卡的太紧，稍微与题解有细节不同，如接收数据不同就会TLE





