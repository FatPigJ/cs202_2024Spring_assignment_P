# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：PyCharm



## 1. 题目

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/



思路：

使用BFS算法

代码

```python
def read():
    return map(int, input().split())

from collections import deque
N = int(input())
tree = [[] for _ in range(N+1)]
is_root = [0] + [1 for _ in range(N)]
for node in range(1, N+1):
    l, r = read()
    if l != -1:
        is_root[l] = 0
        tree[node].append(l)
    if r != -1:
        is_root[r] = 0
        tree[node].append(r)

root = is_root.index(1)
queue = deque([root])
result = []
while queue:
    l = len(queue)
    for i in range(l):
        node = queue.popleft()
        if tree[node]:
            queue.extend(tree[node])
        if i == l-1:
            result.append(node)

print(*result)
```



代码运行截图







### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/



思路：使用单调栈



代码

```python
def read():
    return map(int, input().split())

n = int(input())
arr = list(read())
mono_stack = []
result = []

for i, m in enumerate(arr[::-1]):
    while mono_stack and mono_stack[-1][1] <= m:
        mono_stack.pop()
    result.append(n - mono_stack[-1][0] if mono_stack else 0)
    mono_stack.append((i, m))

print(*reversed(result))
```



代码运行截图





### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/



思路：使用拓扑排序



代码

```python
def read():
    return map(int, input().split())

from collections import deque

def topological_sort(indeg_dic, graph, size):
    queue = deque()
    for vert, indeg in enumerate(indeg_dic):
        if indeg == 0:
            queue.append(vert)
    visited = set()

    while queue:
        vert = queue.popleft()
        visited.add(vert)
        for nbr in graph[vert]:
            if nbr not in visited:
                indeg_dic[nbr] -= 1
                if indeg_dic[nbr] == 0:
                    queue.append(nbr)

    return len(visited) == size


T = int(input())
for _test_case in range(T):
    N, M = read()
    indeg = [1] + [0 for _ in range(N)]
    graph = [[] for _ in range(N+1)]
    for _ in range(M):
        x, y = read()
        graph[x].append(y)
        indeg[y] += 1
    if topological_sort(indeg, graph, N):
        print("No")
    else:
        print("Yes")
```



代码运行截图







### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/



思路：二分查找



代码

```python
def read():
    return map(int, input().split())

def BinarySearch(spending, arr):
    num = 1
    temp = 0
    for amount in arr:
        if temp + amount > spending:
            num += 1
            temp = amount
        else:
            temp += amount
    return num

N, M = read()
arr = []
for _ in range(N):
    arr.append(int(input()))

l = max(arr)
h = sum(arr)
if BinarySearch(l, arr) <= M:
    print(l)
else:
    spending = (l + h) // 2
    while h > l + 1:
        if BinarySearch(spending, arr) <= M:
            h = spending
            spending = (l + h) // 2
        else:
            l = spending
            spending = (l + h) // 2
    print(h)
```



代码运行截图







### 07735: 道路

http://cs101.openjudge.cn/practice/07735/



思路：使用dijkstra算法，维护Visited，当一个城市访问过且之前剩余的金币更多时跳过



代码

```python
def read():
    return map(int, input().split())

K = int(input())
N = int(input())
R = int(input())
Edges = [[] for _ in range(N+1)]
Visited = [float('inf') for _ in range(N+1)]
for _ in range(R):
    S, D, L, T = read()
    Edges[S].append((D, L, T))

from heapq import heappop, heappush, heapify

heap = [(0, 0, 1)]
heapify(heap)
while heap:
    dist, token, city = heappop(heap)
    if city == N:
        print(dist)
        break
    if Visited[city] <= token:
        continue
    Visited[city] = token

    for nbr, l, t in Edges[city]:
        if Visited[nbr] <= token + t:
            continue
        if token + t <= K:
            heappush(heap, (dist + l, token + t, nbr))
else:
    print(-1)
```









### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/



思路：

使用3倍并查集

代码

```python
def read():
    return map(int, input().split())

class DisjointSet:
    def __init__(self, size: int):
        self.size = size
        self.list = [0] + [x for x in range(1, size + 1)]

    def find(self, x):
        if self.list[x] == x:
            return x
        self.list[x] = self.find(self.list[x])
        return self.list[x]

    def linked(self, x, y):
        return self.find(x) == self.find(y)

    def union(self, x, y):
        if not self.linked(x, y):
            self.list[self.find(y)] = self.find(x)


N, K = read()
net = DisjointSet(3*N)
result = 0
for _ in range(K):
    n, x, y = read()
    if n == 1:
        if max(x, y) > N or net.linked(x + N, y) or net.linked(x, y + N):
            result += 1
        else:
            net.union(x, y)
            net.union(x+N, y+N)
            net.union(x+2*N, y+2*N)
    else:
        if max(x, y) > N or net.linked(x, y) or net.linked(x+N, y):
            result += 1
        else:
            net.union(x, y + N)
            net.union(x+N, y + 2*N)
            net.union(y, x + 2*N)
print(result)
```



代码运行截图







## 2. 学习总结和收获

OpenJudge 上 python 3.8 不支持 match case 语法





