# Assignment #B: 图论和树算

Updated 1709 GMT+8 Apr 28, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：PyCharm 2023.1.4 (Professional Edition)

## 1. 题目

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/



思路：dfs



代码

```python
di = [1, 0, -1, 0]; dj = [0, 1, 0, -1]

def dfs(mat, i, j):
    mat[i][j] = '-'
    for k in range(4):
        if 0 <= i+di[k] <= 9 and 0 <= j+dj[k] <= 9 and mat[i+di[k]][j+dj[k]] == '.':
            dfs(mat, i+di[k], j+dj[k])

mat = []
for _i in range(10):
    mat.append(list(input()))
count = 0
for i in range(10):
    for j in range(10):
        if mat[i][j] == '.':
            count += 1
            dfs(mat, i, j)
print(count)
```



代码运行截图 ==（至少包含有"Accepted"）==





### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/



思路：

dfs

代码

```python
arr=[]
def queen(i,b,c,d,string):
    global arr
    for j in range(1,9):
        if j not in b and i-j not in c and i+j not in d:
            if i==8:
                arr.append(string+str(j))
            else:
                queen(i+1,b.union({j}),c.union({i-j}),d.union({i+j}),string+str(j))

queen(1,set(),set(),set(),'')
arr.sort()

n=int(input())
for i in range(n):
    print(arr[int(input())-1])
```



代码运行截图 







### 03151: Pots

bfs, http://cs101.openjudge.cn/practice/03151/



思路：新建表示状态的state类，用bfs算法



代码

```python
from collections import deque
A, B, C = map(int, input().split())
class state:
    def __init__(self, a_vol=0, b_vol=0, opers=''):
        self.a_vol = a_vol
        self.b_vol = b_vol
        self.opers = opers

    def __str__(self):
        return f'({self.a_vol}, {self.b_vol}, {self.opers})'

def fill1(st):
    return state(A, st.b_vol, st.opers + '1')

def fill2(st):
    return state(st.a_vol, B, st.opers + '2')

def drop1(st):
    return state(0, st.b_vol, st.opers + '3')

def drop2(st):
    return state(st.a_vol, 0, st.opers + '4')

def pour12(st):
    if B - st.b_vol >= st.a_vol:
        return state(0, st.a_vol + st.b_vol, st.opers + '5')
    else:
        return state(st.a_vol - (B - st.b_vol), B, st.opers + '5')

def pour21(st):
    if A - st.a_vol >= st.b_vol:
        return state(st.a_vol + st.b_vol, 0, st.opers + '6')
    else:
        return state(A, st.b_vol - (A - st.a_vol), st.opers + '6')

def ended(st):
    return st.a_vol == C or st.b_vol == C

def visited(st, visited_set: set):
    return (st.a_vol, st.b_vol) in visited_set

def output(st):
    __dic = {'1': 'FILL(1)', '2':'FILL(2)', '3': 'DROP(1)', '4': 'DROP(2)', '5': 'POUR(1,2)', '6': 'POUR(2,1)'}
    print(len(st.opers))
    for oper in st.opers:
        print(__dic[oper])

func_list = [fill1, fill2, drop1, drop2, pour12, pour21]
queue = deque()
queue.append(state())
visited_set = {(0, 0)}
while queue:
    st = queue.popleft()
    if ended(st):
        output(st)
        break
    for func in func_list:
        temp = func(st)
        if not visited(temp, visited_set):
            queue.append(temp)
            visited_set.add((temp.a_vol, temp.b_vol))
else:
    print("impossible")
```



代码运行截图







### 05907: 二叉树的操作

http://cs101.openjudge.cn/practice/05907/



思路：定义类tree，用列表保存节点，交换时交换节点在列表中的位置，同时维护字典来按名称查找节点在列表中位置



代码

```python
class Node:
    def __init__(self, value, left, right):
        self.value = value
        self.left = left
        self.right = right

class Tree:
    def __init__(self, n):
        self.list = [None for _i in range(n)]
        self.dic = {i: i for i in range(n)}
        self.n = n

    def add(self, cmd):
        x, y, z = map(int, cmd.split())
        self.list[x] = Node(x, y, z)

    def swap(self, x, y):
        idx_x, idx_y = self.dic[x], self.dic[y]
        self.list[idx_x], self.list[idx_y] = self.list[idx_y], self.list[idx_x]
        self.dic[x] = idx_y
        self.dic[y] = idx_x

    def request(self, x):
        idx_x = self.dic[x]
        while self.list[idx_x].left != -1:
            idx_x = self.list[idx_x].left
        return self.list[idx_x].value

t = int(input())
for _test_case in range(t):
    n, m = map(int, input().split())
    tr = Tree(n)
    for _ in range(n):
        tr.add(input())
    for _ in range(m):
        inp = input()
        if inp[0] == '1':
            _type, x, y = map(int, inp.split())
            tr.swap(x, y)
        else:
            _type, x = map(int, inp.split())
            print(tr.request(x))
```



代码运行截图 









### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/



思路：使用DisjointSet类



代码

```python
class DisjointSet:
    def __init__(self, n):
        self.n = n
        self.list = [i for i in range(n)]

    def is_root(self, x):
        return self.list[x] == x

    def find(self, x):
        if self.list[x] != x:
            self.list[x] = self.find(self.list[x])
        return self.list[x]

    def is_linked(self, x, y):
        return self.find(x) == self.find(y)

    def union(self, x, y):
        self.list[self.find(y)] = self.find(x)

    def output(self):
        count = 0
        ans = []
        for x in range(self.n):
            if self.is_root(x):
                count += 1
                ans.append(x+1)
        print(count)
        print(*sorted(ans))


while 1:
    try:
        n, m = map(int, input().split())
        d_j = DisjointSet(n)
        for _ in range(m):
            x, y = map(int, input().split())
            if d_j.is_linked(x-1, y-1):
                print("Yes")
            else:
                d_j.union(x-1, y-1)
                print('No')
        d_j.output()
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==







### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/



思路：dijkstra算法



代码

```python
from heapq import heappop, heappush, heapify
class Vertex:
    def __init__(self, key=''):
        self.key = key
        self.neighbors = {}
        self.distance = float('inf')
        self.prev = None

    def __str__(self):
        return str(self.key)

    def __bool__(self):
        return True

    def add_neighbor(self, nbr, dis: int):
        self.neighbors[nbr] = dis

    def get_weight(self, nbr):
        return self.neighbors[nbr]

class Graph:
    def __init__(self):
        self.vert_list = {}
        self.size = 0

    def get(self, key):
        return self.vert_list[key]

    def add_vertex(self, key):
        v = Vertex(key)
        self.vert_list[v.key] = v
        self.size += 1

    def add_edge(self, k1, k2, dis: int):
        if k1 not in self.vert_list:
            self.add_vertex(k1)
        if k2 not in self.vert_list:
            self.add_vertex(k2)
        v1 = g.get(k1)
        v2 = g.get(k2)
        v1.add_neighbor(v2, dis)
        v2.add_neighbor(v1, dis)

    def flush(self):
        for v in self.vert_list.values():
            v.distance = float('inf')
            v.prev = None

def dijkstra(g: Graph, start, end):
    heap = [(0, g.get(start))]
    heapify(heap)
    visited = set()
    while 1:
        dist, v = heappop(heap)
        if v.key == end:
            break
        if v.key in visited:
            continue
        visited.add(v.key)

        for nbr, w in v.neighbors.items():
            if nbr.key not in visited and dist + w < nbr.distance:
                nbr.distance = dist + w
                nbr.prev = v
                heappush(heap, (nbr.distance, nbr))

def output(g: Graph, end):
    ans = [end]
    v = g.get(end)
    while v.prev:
        ans.append(f'({v.prev.get_weight(v)})')
        ans.append(v.prev.key)
        v = v.prev
    return '->'.join(reversed(ans))


g = Graph()
P = int(input())
for _ in range(P):
    g.add_vertex(input())
Q = int(input())
for _ in range(Q):
    k1, k2, w = input().split()
    g.add_edge(k1, k2, int(w))
R = int(input())
for _ in range(R):
    g.flush()
    start, end = input().split()
    dijkstra(g, start, end)
    print(output(g, end))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==







## 2. 学习总结和收获

冰阔落前期因为将yes，no大写而WA，以后应更细心一点





