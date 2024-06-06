# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：PyCharm



## 1. 题目

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现



思路：新建Vertex类，Graph类



代码

```python
class Vertex(object):
    def __init__(self, key=None):
        self.key = key
        self.neighbours = set()

    def __str__(self):
        return str(self.key)

    def add_neighbour(self, nbr):
        self.neighbours.add(nbr)

    def degree(self):
        return len(self.neighbours)


class Graph(object):
    def __init__(self):
        self.verts = {}

    def add_vertex(self, key):
        self.verts[key] = Vertex(key)

    def add_edge(self, key1, key2):
        if key1 in self.verts.keys():
            vert1 = self.verts[key1]
        else:
            vert1 = Vertex(key1)
            self.verts[key1] = vert1
        if key2 in self.verts.keys():
            vert2 = self.verts[key2]
        else:
            vert2 = Vertex(key2)
            self.verts[key2] = vert2

        vert1.add_neighbour(vert2)
        vert2.add_neighbour(vert1)

    def size(self):
        return len(self.verts)

    def laplace_mat(self):
        n = self.size()
        mat = [[0 for j in range(n)] for i in range(n)]
        for i in range(n):
            for j in range(n):
                if i == j:
                    mat[i][j] = self.verts[i].degree()
                else:
                    mat[i][j] = -int(self.verts[i] in self.verts[j].neighbours)

        return mat

g = Graph()
n, m = map(int, input().split())
for i in range(n):
    g.add_vertex(i)
for i in range(m):
    g.add_edge(*map(int, input().split()))
l = g.laplace_mat()
for i in l:
    print(*i)
```



代码运行截图







### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160



思路：

使用dfs

代码

```python
mat = [[]]
di = [-1, -1, -1, 0, 0, 1, 1, 1]; dj = [-1, 0, 1, -1, 1, -1, 0, 1]

def dfs(i, j):
    global mat
    mat[i][j] = '.'
    area = 1
    for k in range(8):
        if mat[i + di[k]][j + dj[k]] == 'W':
            area += dfs(i + di[k], j + dj[k])
    return area


T = int(input())
for test_case in range(T):
    N, M = map(int, input().split())
    mat = [['.' for j in range(M+2)]]
    for i in range(N):
        mat.append(['.']+list(input())+['.'])
    mat.append(['.' for j in range(M+2)])
    maxa = 0
    for i in range(1, N+1):
        for j in range(1, M+1):
            if mat[i][j] == 'W':
                area = dfs(i, j)
                if area > maxa:
                    maxa = area
    print(maxa)
```



代码运行截图







### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383



思路：使用disjoint set



代码

```python
def find(i: int):
    if roots[i] == i:
        return i
    return find(roots[i])


n, m = map(int, input().split())
roots = [i for i in range(n)]
size = list(map(int, input().split()))
for edge in range(m):
    v1, v2 = map(int, input().split())
    r1 = find(v1); r2 = find(v2)
    roots[r2] = r1
    if r1 != r2:
        size[r1] += size[r2]

print(max(size))
```



代码运行截图 







### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441



思路：使用dic存储两个list笛卡尔积的和



代码

```python
from collections import defaultdict
n = int(input())
dic1 = {}
dic2 = {}
list1, list2, list3, list4 = [], [], [], []
for _ in range(n):
    a, b, c, d = map(int, input().split())
    list1.append(a)
    list2.append(b)
    list3.append(c)
    list4.append(d)
for a in list1:
    for b in list2:
        dic1[a+b] = dic1.get(a+b, 0) + 1
ans = 0
for c in list3:
    for d in list4:
        ans += dic1.get(-c-d, 0)
print(ans)
```



代码运行截图







### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。



思路：

使用树结构，第一层为第一个数字，等等。将电话号码按长度从小到大排列，如果一个电话号码的路径经过其他电话号码则为NO

代码

```python
class Node(object):
    def __init__(self):
        self.type = 1
        self.children = [None for i in range(10)]

    def __bool__(self):
        return True

t = int(input())
for test_case in range(t):
    root = Node()
    n = int(input())
    flag = 1
    Tels = []

    for _ in range(n):
        Tels.append(list(map(int, input())))
    
    Tels.sort()
    for tel in Tels:
        node = root

        for num in tel:
            if node.children[num]:
                if node.children[num].type:
                    node = node.children[num]
                else:
                    flag = 0
                    break
            else:
                node.children[num] = Node()
                node = node.children[num]

        if flag:
            node.type = 0
        else:
            break

    if flag:
        print("YES")
    else:
        print('NO')
```



代码运行截图 







### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/



思路：定义parse_tree将遍历转化为二叉树，binary_to将二叉树复原，traversal输出BFS遍历，其中完成镜面反射



代码

```python
from collections import deque
class Node(object):
    def __init__(self, value=None, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right
        self.children = []

    def __str__(self):
        return str(self.value)

    def __bool__(self):
        return True


def parse_tree(L: list, idx: int):
    if L[idx][0] == '$':
        return None, idx+1
    if L[idx][1] == '1':
        return Node(L[idx][0]), idx+1

    left, right_idx = parse_tree(L, idx+1)
    right, next_idx = parse_tree(L, right_idx)
    return Node(L[idx][0], left, right), next_idx

def binary_to(b_root: Node):
    root = Node(b_root.value)
    if b_root.left:
        node = b_root.left
        children = [node]
        while node.right:
            children.append(node.right)
            node = node.right
        for child in children:
            root.children.append(binary_to(child))
    return root


def traversal(root: Node):
    output = []
    queue = deque()
    queue.append(root)

    while queue:
        node = queue.popleft()
        output.append(node.value)
        queue += list(reversed(node.children))

    return output


n = int(input())
L = input().split()
b_root, _ = parse_tree(L, 0)
root = binary_to(b_root)
print(*traversal(root))
```



代码运行截图







## 2. 学习总结和收获

最后一题树的问题还是很有难度的





