# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**



操作系统：Windows 11

Python编程环境：PyCharm



## 1. 题目

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：

使用reversed函数

代码

```python
print(*reversed(input().split()))
```



代码运行截图 ==（至少包含有"Accepted"）==







### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：使用deque，调用一个列表来记录内存中是否有某单词



代码

```python
from collections import deque
M, N = map(int, input().split())
fltr = [0 for i in range(1002)]
l = 0
queue = deque()
count = 0
words = list(map(int, input().split()))
for w in words:
    if not fltr[w]:
        count += 1
        if l < M:
            queue.append(w)
            l += 1
            fltr[w] = 1
        else:
            fltr[queue.popleft()] = 0
            queue.append(w)
            fltr[w] = 1
print(count)
```



代码运行截图 ==（至少包含有"Accepted"）==





### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：

k == 0, k==n单独讨论

代码

```python
n, k = map(int, input().split())
arr = sorted(list(map(int, input().split())))
if k == 0:
    if arr[0] > 1:
        print(1)
    else:
        print(-1)
elif k < n and arr[k] == arr[k-1]:
    print(-1)
else:
    print(arr[k-1])
```



代码运行截图





### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：

在解析树时同步更新节点的类型

代码

```python
class Node(object):
    def __init__(self, type=None):

        self.type = type
        self.left = None
        self.right = None

    def  __bool__(self):
        return True


def parse(string, l, r):
    if l == r:
        return Node({'0':'B', '1':'I'}[string[l]])
    else:
        node = Node()
        node.left = parse(string, l, (l+r)//2)
        node.right = parse(string, (l+r)//2 + 1, r)
        if node.left.type == node.right.type == 'B':
            node.type = 'B'
        elif node.left.type == node.right.type == 'I':
            node.type = 'I'
        else:
            node.type = 'F'
        return node


def traversal(root: Node):
    return (traversal(root.left) if root.left else '') + (traversal(root.right) if root.right else '') + root.type


N = int(input())
string = input()
root = parse(string, 0, 2**N-1)
print(traversal(root))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：

自定义类group_queue, 实现enqueue, dequeue等操作

代码

```python
from collections import deque

class group_queue(object):
    def __init__(self, n: int, dic: dict):
        self.n = n
        self.dic = dic
        self.queue = [deque() for i in range(self.n)]
        self.groups = deque()
        self.is_empty = [1 for i in range(self.n)]

    def enqueue(self, member: int):
        group = self.dic[member]
        if self.is_empty[group]:
            self.is_empty[group] = 0
            self.queue[group].append(member)
            self.groups.append(group)
        else:
            self.queue[group].append(member)

    def dequeue(self):
        group = self.groups[0]
        member = self.queue[group].popleft()
        if not self.queue[group]:
            self.is_empty[group] = 1
            self.groups.popleft()
        return member

t = int(input())
dic = {}
for group in range(t):
    for member in map(int, input().split()):
        dic[member] = group
g_q = group_queue(t, dic)

while 1:
    inp = input()
    if inp[0] == 'E':
        g_q.enqueue(int(inp.split()[-1]))
    elif inp[0] == 'D':
        print(g_q.dequeue())
    else:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：

使用字典辅助建树

代码

```python
from functools import reduce
from operator import add

class Node(object):
    def __init__(self, value=None):
        self.value = value
        self.children = []

    def __bool__(self):
        return True

    def __lt__(self, other):
        return self.value <= other.value

    def traversal(self):
        if not self.children:
            return [self.value]
        else:
            return reduce(add, (node.traversal() for node in sorted(self.children + [Node(self.value)])))


dic = {}
is_root = {}
n = int(input())
for _ in range(n):
    inp = list(map(int, input().split()))

    if inp[0] in dic.keys():
        node = dic[inp[0]]
    else:
        node = Node(inp[0])
        dic[inp[0]] = node

    if inp[0] not in is_root.keys():
        is_root[inp[0]] = 1

    for num in inp[1:]:
        is_root[num] = 0
        if num in dic.keys():
            node.children.append(dic[num])
        else:
            child = Node(num)
            node.children.append(child)
            dic[num] = child

for num,b in is_root.items():
    if b:
        root = dic[num]
        break

print(*root.traversal(), sep='\n')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==







## 2. 学习总结和收获

学会了使用

```py
from functools import reduce
from operator import add
reduce(add, (list1, list2, list3))
```

合并多个列表



