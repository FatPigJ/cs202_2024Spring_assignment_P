# Assignment #9: 图论：遍历，及 树算

Updated 1739 GMT+8 Apr 14, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：Pycharm



## 1. 题目

### 04081: 树的转换

http://cs101.openjudge.cn/dsapre/04081/



思路：定义了两个类，一般的树和二叉树



代码

```python
class Node(object):
    def __init__(self):
        self.children = []

    def __bool__(self):
        return True

    def height(self):
        if self.children:
            return 1 + max(child.height() for child in self.children)
        else:
            return 0
class BiNode(object):
    def __init__(self, left=None, right=None):
        self.left = left
        self.right = right

    def __bool__(self):
        return True

    def height(self):
        return max(self.left.height() + 1 if self.left else 0, self.right.height() + 1 if self.right else 0)

def parse(string: str) -> Node :
    root = Node()
    node = root
    stack = []
    for char in string:
        if char == 'd':
            child = Node()
            node.children.append(child)
            stack.append(node)
            node = child
        else:
            node = stack.pop()
    return root

def convert(root: Node) -> BiNode :
    biroot = BiNode()
    temp = None
    for node in reversed(root.children):
        binode = convert(node)
        binode.right = temp
        temp = binode
    biroot.left = temp
    return biroot

root = parse(input())
h = root.height()
biroot = convert(root)
b_h = biroot.height()
print(f"{h} => {b_h}")
```



代码运行截图







### 08581: 扩展二叉树

http://cs101.openjudge.cn/dsapre/08581/



思路：

从后往前解析树



代码

```python
class Node(object):
    def __init__(self, value='', left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

    def __str__(self):
        return str(self.value)

    def __bool__(self):
        return True

def parse(string: str):
    stack = []
    for char in string[::-1]:
        if char == '.':
            stack.append(None)
        else:
            stack.append(Node(char, stack.pop(), stack.pop()))
    return stack[-1]

def inorder(root: Node):
    return (inorder(root.left) if root.left else '') + root.value + (inorder(root.right) if root.right else '')

def postorder(root: Node):
    return (postorder(root.left) if root.left else '') + (postorder(root.right) if root.right else '') + root.value

root = parse(input())
print(inorder(root))
print(postorder(root))
```



代码运行截图







### 22067: 快速堆猪

http://cs101.openjudge.cn/practice/22067/



思路：维护一个栈，一个堆，另将弹出的猪保存在popped堆里，求最轻猪时检查是否已弹出



代码

```python
from heapq import heappop, heappush, heapify

heap = []
heapify(heap)
stack = []
popped = []
heapify(popped)

while 1:
    try:
        inp = input()
        if inp[1] == 'u':
            pig = int(inp.split()[-1])
            heappush(heap, pig)
            stack.append(pig)
        elif inp[1] == 'i':
            if len(stack) > 0:
                while len(popped) > 0 and heap[0] == popped[0]:
                    heappop(heap)
                    heappop(popped)
                print(heap[0])
        else:
            if len(stack) > 0:
                heappush(popped, stack.pop())
    except EOFError:
        break
```

使用defaultdict 懒删除

```py
from heapq import heappush, heappop, heapify
from collections import defaultdict
heap = []; heapify(heap)
stack= []
dic = defaultdict(int)

while True:
    try:
        inp = input()
        if inp[1] == 'u':
            _, pig = inp.split()
            pig = int(pig)
            stack.append(pig)
            heappush(heap, pig)

        elif inp[1] == 'o' and stack:
            dic[stack.pop()] += 1

        elif inp[1] == 'i' and stack:
            while dic[heap[0]]:
                dic[heappop(heap)] -= 1
            print(heap[0])

    except EOFError:
        break
```
每次push时，在辅助栈中加入当前最轻的猪的体重，pop时也同步pop，这样栈顶始终是当前猪堆中最轻的体重，查询时直接输出即可

```py
stack = []
min_stack = []
while True:
    try:
        inp = input()
        if inp[1] == 'u':
            _, pig = inp.split()
            pig = int(pig)
            stack.append(pig)
            if min_stack:
                min_stack.append(min(min_stack[-1], pig))
            else:
                min_stack.append(pig)

        elif inp[1] == 'o' and stack:
            stack.pop()
            min_stack.pop()

        elif inp[1] == 'i' and stack:
            print(min_stack[-1])

    except EOFError:
        break
```



代码运行截图 







### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123



思路：

使用dfs，vert添加visited属性

代码

```python
from collections import deque
class Vertex(object):
    def __init__(self, key=''):
        self.key = key
        self.neighbors = set()
        self.prev = None
        self.visited = False

    def __str__(self):
        return str(self.key)

    def add_neighbor(self, nbr):
        self.neighbors.add(nbr)

class Graph(object):
    def __init__(self):
        self.size = 0
        self.vertlist = {}

    def add_vertex(self, key):
        self.vertlist[key] = Vertex(key)
        self.size += 1

    def add_edge(self, key1, key2):
        if key1 not in self.vertlist:
            self.add_vertex(key1)
        if key2 not in self.vertlist:
            self.add_vertex(key2)
        self.vertlist[key1].add_neighbor(self.vertlist[key2])
        self.vertlist[key2].add_neighbor(self.vertlist[key1])

def build_graph(n: int, m: int):
    G = Graph()
    dx = [2, 1, -1, -2, -2, -1, 1, 2]
    dy = [1, 2, 2, 1, -1, -2, -2, -1]
    for x in range(n):
        for y in range(m):
            for k in range(8):
                if 0 <= x + dx[k] <= n-1 and 0 <= y + dy[k] <= m-1:
                    G.add_edge((x, y), (x + dx[k], y + dy[k]))
    return G

def dfs(vert: Vertex, G: Graph, n: int, size: int):
    if n == size:
        return 1
    else:
        ans = 0
        for nbr in vert.neighbors:
            if not nbr.visited:
                nbr.visited = True
                ans += dfs(nbr, G, n+1, size)
                nbr.visited = False
        return ans

T = int(input())
for test_case in range(T):
    n, m, x, y = map(int, input().split())
    G = build_graph(n, m)
    vert = G.vertlist[(x, y)]
    vert.visited = True
    ans = dfs(vert, G, 1, n*m)
    print(ans)
```



代码运行截图







### 28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/



思路：

使用题解思路

代码

```python
from collections import deque
class Vertex(object):
    def __init__(self, key=''):
        self.key = key
        self.neighbors = set()
        self.prev = None
        self.visited = False

    def __str__(self):
        return str(self.key)

    def add_neighbor(self, nbr):
        self.neighbors.add(nbr)

class Graph(object):
    def __init__(self):
        self.size = 0
        self.vertlist = {}

    def add_vertex(self, key: str):
        self.vertlist[key] = Vertex(key)
        self.size += 1

    def add_edge(self, key1, key2):
        if key1 not in self.vertlist:
            self.add_vertex(key1)
        if key2 not in self.vertlist:
            self.add_vertex(key2)
        self.vertlist[key1].add_neighbor(self.vertlist[key2])
        self.vertlist[key2].add_neighbor(self.vertlist[key1])

def build_graph(L: list):
    G = Graph()
    buckets = {}

    for word in L:
        for _ in range(4):
            bucket = word[:_] + '_' + word[_+1:]
            buckets.setdefault(bucket, []).append(word)

    for words in buckets.values():
        for i, word1 in enumerate(words):
            for word2 in words[i+1:]:
                G.add_edge(word1, word2)

    return G

def bfs(G: Graph, start: str, end: str):
    queue = deque()
    vert = G.vertlist[start]
    vert.visited = True
    queue.append(vert)

    while 1:
        if not queue:
            break
        vert = queue.popleft()
        if vert.key == end:
            return vert
        for nbr in vert.neighbors:
            if not nbr.visited:
                nbr.prev = vert
                nbr.visited = True
                queue.append(nbr)

    return False

def traversal(end: Vertex):
    ans = []
    vert = end
    ans.append(vert.key)
    while vert.prev:
        vert = vert.prev
        ans.append(vert.key)
    return ' '.join(reversed(ans))


n = int(input())
L = []
for i in range(n):
    L.append(input())
G = build_graph(L)
start, end = input().split()
if bfs(G, start, end):
    print(traversal(G.vertlist[end]))
else:
    print("NO")
```



代码运行截图







### 28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/



思路：

使用Warnsdorff算法，将节点按avai_deg可访问的邻居数升序排列

代码

```python
from collections import deque
class Vertex(object):
    def __init__(self, key=''):
        self.key = key
        self.neighbors = set()
        self.prev = None
        self.visited = False

    def __str__(self):
        return str(self.key)

    def add_neighbor(self, nbr):
        self.neighbors.add(nbr)

    def avai_deg(self):
        avai_deg = 0
        for nbr in self.neighbors:
            avai_deg += int(not nbr.visited)
        return avai_deg

    def __lt__(self, other):
        return self.avai_deg() < other.avai_deg()

class Graph(object):
    def __init__(self):
        self.size = 0
        self.vertlist = {}

    def add_vertex(self, key):
        self.vertlist[key] = Vertex(key)
        self.size += 1

    def add_edge(self, key1, key2):
        if key1 not in self.vertlist:
            self.add_vertex(key1)
        if key2 not in self.vertlist:
            self.add_vertex(key2)
        self.vertlist[key1].add_neighbor(self.vertlist[key2])
        self.vertlist[key2].add_neighbor(self.vertlist[key1])

def build_graph(n: int, m: int):
    G = Graph()
    dx = [2, 1, -1, -2, -2, -1, 1, 2]
    dy = [1, 2, 2, 1, -1, -2, -2, -1]
    for x in range(n):
        for y in range(m):
            for k in range(8):
                if 0 <= x + dx[k] <= n-1 and 0 <= y + dy[k] <= m-1:
                    G.add_edge((x, y), (x + dx[k], y + dy[k]))
    return G

def dfs(vert: Vertex, G: Graph, n: int, size: int):
    if n == size:
        return True
    else:
        for nbr in sorted(vert.neighbors):
            if not nbr.visited:
                nbr.visited = True
                if dfs(nbr, G, n+1, size):
                    return True
                nbr.visited = False
        return False


n = int(input())
x, y = map(int, input().split())
G = build_graph(n, n)
vert = G.vertlist[(x, y)]
vert.visited = True
ans = dfs(vert, G, 1, n*n)
print({True: 'success', False: 'fail'}[ans])
```



代码运行截图







## 2. 学习总结和收获

使用合适的算法可以大幅度提高运行效率





