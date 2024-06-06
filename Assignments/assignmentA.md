# Assignment #A: 图论：遍历，树算及栈

Updated 2018 GMT+8 Apr 21, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：PyCharm



## 1. 题目

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/



思路：参照题解，维护栈



代码

```python
def parse(string: str):
    stack = []
    for char in string:
        if char == ')':
            temp = []
            while stack[-1] != '(':
                temp.append(stack.pop())
            stack.pop()
            stack.extend(temp)
        else:
            stack.append(char)
    return ''.join(stack)

print(parse(input()))
```



代码运行截图







### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/



思路：定义parse函数，后序遍历函数



代码

```python
class Node:
    def __init__(self, value='', left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

    def __str__(self):
        return str(self.value)

    def __bool__(self):
        return True

def parse(pre_order: str, in_order: str):
    if pre_order == '':
        return None

    root_value = pre_order[0]
    idx = in_order.find(root_value)
    return Node(root_value, parse(pre_order[1:idx+1], in_order[:idx]), parse(pre_order[idx+1:], in_order[idx+1:]))

def post_order_traversal(root: Node):
    return (post_order_traversal(root.left) if root.left else '') + (post_order_traversal(root.right) if root.right else '') + root.value


while 1:
    try:
        pre_order, in_order = input().split()
        root = parse(pre_order, in_order)
        print(post_order_traversal(root))
    except EOFError:
        break
```



代码运行截图







### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现



思路：bfs，第一轮单独讨论，因为不能结果为0



代码

```python
from collections import deque
ran = [0,1,2,3,4,5,6,7,8,9]

def bfs(n: int):
    queue = deque()
    m = 0; deg = 1; r = 0
    for i in ran[1:]:
        if (i * n + r) % 10 in {0, 1}:
            k = m + i * n * (10 ** (deg - 1))
            if set(str(k)) - {'0', '1'} == set():
                return k
            queue.append((k, deg + 1))
    while 1:
        m, deg = queue.popleft()
        r = int(str(m)[-deg])
        for i in ran:
            if (i * n + r) % 10 in {0,1}:
                k = m+i*n*(10**(deg-1))
                if set(str(k)) - {'0', '1'} == set():
                    return k
                queue.append((k, deg+1))

while 1:
    n = int(input())
    if n == 0:
        break
    print(bfs(n))
```



代码运行截图







### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/



思路：使用bfs，令有visited字典保存走过位置的最大chakra，防止冗余



代码

```python
from collections import deque
di = [1,0,0,-1]; dj = [0,1,-1,0]

def bfs(mat, M, N, T):
    found = 0
    for i in range(M):
        if found:
            break
        for j in range(N):
            if mat[i][j] == '@':
                naruto_i = i
                naruto_j = j
                found = 1
                break

    if mat[naruto_i][naruto_j] == '+':
        return 0

    queue = deque([(naruto_i, naruto_j, T)])
    step = 0
    visited = {(naruto_i, naruto_j): T}
    while queue:
        step += 1
        for _ in range(len(queue)):
            i, j, chakra = queue.popleft()
            for k in range(4):
                if 0<=i+di[k]<M and 0<=j+dj[k]<N and (chakra or mat[i+di[k]][j+dj[k]] != '#') and visited.get((i+di[k], j+dj[k]), -1) < chakra:
                    if mat[i+di[k]][j+dj[k]] == '+':
                        return step
                    queue.append((i+di[k], j+dj[k], chakra - int(mat[i+di[k]][j+dj[k]] == '#')))
                    visited[(i+di[k], j+dj[k])] = chakra - int(mat[i+di[k]][j+dj[k]] == '#')
    return -1

M, N, T = map(int, input().split())
mat = []
for i in range(M):
    mat.append(list(input()))
ans = bfs(mat, M, N, T)
print(ans)
```



代码运行截图







### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



思路：

使用dijkstra算法

代码

```python
from heapq import heappop, heappush, heapify
di = [1,0,0,-1]; dj = [0,1,-1,0]

def dijkstra(mat, M, N, start, end):
    dist_mat = [[float('inf') for j in range(N)] for i in range(M)]
    hp = [(0,) + start]
    heapify(hp)
    visited = set()
    while hp:
        dist, i, j = heappop(hp)
        if (i, j) in visited:
            continue
        visited.add((i, j))
        if (i, j) == end:
            return dist
        for k in range(4):
            if 0<=i+di[k]<M and 0<=j+dj[k]<N and mat[i+di[k]][j+dj[k]] != '#' and (i+di[k], j+dj[k]) not in visited:
                weight = abs(int(mat[i][j]) - int(mat[i+di[k]][j+dj[k]]))
                newdist = dist + weight
                if newdist < dist_mat[i+di[k]][j+dj[k]]:
                    dist_mat[i + di[k]][j + dj[k]] = newdist
                    heappush(hp, (newdist, i+di[k], j+dj[k]))
    return 'NO'

M, N, p = map(int, input().split())
mat = []
for row in range(M):
    mat.append(input().split())
for test in range(p):
    i1, j1, i2, j2 = map(int, input().split())
    if mat[i1][j1] == '#' or mat[i2][j2] == '#':
        print("NO")
        continue
    else:
        print(dijkstra(mat, M, N, (i1, j1), (i2, j2)))
```



代码运行截图 







### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/



思路：

使用并查集，kruskal算法

代码

```python
class DisjointSet:
    def __init__(self, vert_list: list):
        self.list = vert_list
        self.rank = [0 for v in vert_list]

    def find(self, x):
        if self.list[x] != x:
            return self.find(self.list[x])
        return x

    def linked(self, x, y):
        return self.find(x) == self.find(y)

    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root != y_root:
            if self.rank[x_root] > self.rank[y_root]:
                self.list[y_root] = x_root
            elif self.rank[x_root] < self.rank[y_root]:
                self.list[x_root] = y_root
            else:
                self.list[y_root] = x_root
                self.rank[x_root] += 1

def kruskal(edges: list, vert_list: list):
    d_s = DisjointSet(vert_list)
    edges.sort()
    weight = 0
    for w, x, y in edges:
        if not d_s.linked(x, y):
            weight += w
            d_s.union(x, y)
    return weight

def N(alpha: str):
    return ord(alpha) - 65


n = int(input())
vert_list = list(range(n))
edges = []
for x in range(n-1):
    inp = input().split()
    for i in range(2, len(inp), 2):
        edges.append((int(inp[i+1]), x, N(inp[i])))
weight = kruskal(edges, vert_list)
print(weight)
```



代码运行截图







## 2. 学习总结和收获

在bfs，dijkstra程序中每一个节点通过小循环找邻居时常常因为在小循环中改了节点的相关值导致出错，以后要多加注意





