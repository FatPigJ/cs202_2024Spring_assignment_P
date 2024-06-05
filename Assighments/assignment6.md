# Assignment #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

Updated 2214 GMT+8 March 24, 2024



**说明：**

1）这次作业内容不简单，耗时长的话直接参考题解。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：PyCharm



## 1. 题目

### 22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/



思路：前序遍历中根节点下一个为左，第一个大于根节点的为右



代码

```python
import sys
sys.setrecursionlimit(1000000)
class Node(object):
    def __init__(self, value=None, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

    def __str__(self):
        return str(self.value)

    def __bool__(self):
        return True

def parse(root: Node, preorder: list):
    root.value = preorder[0]
    l = len(preorder)
    if l > 1:
        if preorder[1] > root.value:
            right = Node()
            root.right = right
            parse(right, preorder[1:])

        else:
            left = Node()
            root.left = left
            for i in range(1, l+1):
                if i < l and preorder[i] > root.value:
                    right = Node()
                    root.right = right
                    parse(right, preorder[i:])
                    break
            parse(left, preorder[1:i])

def postorder_traversal(root: Node):
    return (postorder_traversal(root.left) if root.left else []) + (postorder_traversal(root.right) if root.right else []) + [root.value]

n = int(input())
root = Node()
preorder = list(map(int, input().split()))
parse(root, preorder)
print(*postorder_traversal(root))
```



代码运行截图







### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/



思路：从根出发，判断数值大于/小于根来确定插入位置



代码

```python
from collections import deque
class Node(object):
    def __init__(self, value=None):
        self.value = value
        self.left = None
        self.right = None

    def __str__(self):
        return str(self.value)

    def __bool__(self):
        return True

def insert(root: Node, num: int):
    node = root
    while 1:
        if num == node.value:
            return
        elif num < node.value:
            if not node.left:
                node.left = Node(num)
                break
            else:
                node = node.left
        else:
            if not node.right:
                node.right = Node(num)
                break
            else:
                node = node.right

def traversal(root: Node):
    queue = deque([root])
    output = []
    while queue:
        node = queue.popleft()
        output.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return output

inp = list(map(int, input().split()))
root = Node(inp[0])
for num in inp[1:]:
    insert(root, num)
print(*traversal(root))
```



代码运行截图 







### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。



思路：

使用二叉堆 BinHeap class

代码

```python
class BinHeap(object):
    def __init__(self):
        self.arr = [0]
        self.size = 0

    def up_perc(self, i):
        while i > 1:
            if self.arr[i//2] > self.arr[i]:
                self.arr[i//2], self.arr[i] = self.arr[i], self.arr[i//2]
                i //= 2
            else:
                break

    def down_perc(self, i):
        while i <= self.size//2:
            if 2*i+1 > self.size:
                if self.arr[2*i] < self.arr[i]:
                    self.arr[2*i], self.arr[i] = self.arr[i], self.arr[2*i]
                    break
                else:
                    break
            else:
                if min(self.arr[2*i+1], self.arr[2*i]) < self.arr[i]:
                    if self.arr[2*i+1] <= self.arr[2*i]:
                        self.arr[2 * i + 1], self.arr[i] = self.arr[i], self.arr[2 * i + 1]
                        i = 2*i+1
                    else:
                        self.arr[2 * i], self.arr[i] = self.arr[i], self.arr[2 * i]
                        i *= 2
                else:
                    break

    def insert(self, num):
        self.arr.append(num)
        self.size += 1
        self.up_perc(self.size)

    def pop(self):
        if self.size > 1:
            popped = self.arr[1]
            self.arr[1] = self.arr.pop()
            self.size -= 1
            self.down_perc(1)
            return popped
        else:
            self.size -= 1
            return self.arr.pop()

n = int(input())
heap = BinHeap()
for _ in range(n):
    inp = input()
    if inp[0] == '1':
        heap.insert(int(list(inp.split())[-1]))
    else:
        print(heap.pop())
```



代码运行截图 







### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/



思路：定义node，按定义实现huffman树



代码

```python
from heapq import heapify, heappop, heappush

class Node(object):
    def __init__(self, freq=float('inf'), left=None, right=None, value='猪'):
        self.value = value
        self.left = left
        self.right = right
        self.freq = freq

    def __str__(self):
        return self.freq

    def __lt__(self, other):
        return [self.freq, self.value] < [other.freq, other.value]

    def decode_map(self, prev=''):
        return {prev: self.value} if self.value != '猪' else dict(list(self.left.decode_map(prev + '0').items())+list(self.right.decode_map(prev + '1').items()))

    def encode_map(self, prev=''):
        return {self.value: prev} if self.value != '猪' else dict(list(self.left.encode_map(prev + '0').items())+list(self.right.encode_map(prev + '1').items()))



def build(dic: dict):
    heap = [Node(f, value=c) for c, f in dic.items()]
    heapify(heap)

    while len(heap)>1:
        node1 = heappop(heap)
        node2 = heappop(heap)
        heappush(heap, Node(node1.freq + node2.freq, node1, node2))

    return heappop(heap)

n = int(input())
dic = {char: int(freq) for char, freq in [input().split() for i in range(n)]}
root = build(dic)
encode = root.encode_map()
decode = root.decode_map()
code = set(decode.keys())

while 1:
    try:
        string = input()
        if string.isalpha():
            print(''.join([encode[char] for char in string]))
        else:
            output = ''
            temp = ''
            for char in string:
                temp += char
                if temp in code:
                    output += decode[temp]
                    temp = ''

            print(output)
    except EOFError:
        break
```



代码运行截图







### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359



思路：使用Node class，使用_height_refresh(), _balance_refresh()方法更新各节点的平衡



代码

```python
class Node(object):
    def __init__(self, value=None):
        self.value = value
        self.left = None
        self.right = None
        self.height = 1
        self.balance = 0

    def __str__(self):
        return str(self.value)

    def __bool__(self):
        return True

    def _height_refresh(self):
        if self.left or self.right:
            self.height = 1 + max((self.left.height if self.left else 0), (self.right.height if self.right else 0))
        else:
            self.height = 1

    def _balance_refresh(self):
        self.balance = (self.left.height if self.left else 0) - (self.right.height if self.right else 0)

    def _right_rotate(self):
        b = self.left
        f = b.right
        self.left = f
        b.right = self
        self._height_refresh()
        self._balance_refresh()
        b._height_refresh()
        b._balance_refresh()
        return b

    def _left_rotate(self):
        b = self.right
        f = b.left
        self.right = f
        b.left = self
        self._height_refresh()
        self._balance_refresh()
        b._height_refresh()
        b._balance_refresh()
        return b

    def insert(self, num):
        if num == self.value:
            return self
        elif num < self.value:
            if self.left:
                self.left = self.left.insert(num)
            else:
                self.left = Node(num)
        else:
            if self.right:
                self.right = self.right.insert(num)
            else:
                self.right = Node(num)

        self._height_refresh()
        self._balance_refresh()
        if self.balance > 1:
            if self.left.balance > 0:
                return self._right_rotate()
            else:
                self.left = self.left._left_rotate()
                return self._right_rotate()
        elif self.balance < -1:
            if self.right.balance < 0:
                return self._left_rotate()
            else:
                self.right = self.right._right_rotate()
                return self._left_rotate()
        else:
            return self


    def preorder_traversal(self):
        return [self.value] + (self.left.preorder_traversal() if self.left else []) + (self.right.preorder_traversal() if self.right else [])

n = int(input())
arr = list(map(int, input().split()))
root = Node(arr[0])
for num in arr[1:]:
    root = root.insert(num)
print(*root.preorder_traversal())
```



代码运行截图 







### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/



思路：参照题解



代码

```python
 

def get_father(x, father):
    if father[x] != x:
        father[x] = get_father(father[x], father)
    return father[x]

def join(x, y, father):
    fx = get_father(x, father)
    fy = get_father(y, father)
    if fx == fy:
        return
    father[fx] = fy

def is_same(x, y, father):
    return get_father(x, father) == get_father(y, father)


case_num = 0
while True:
    n, m = map(int, input().split())
    if n == 0 and m == 0:
        break
    count = 0
    father = list(range(n))
    for _ in range(m):
        s1, s2 = map(int, input().split())
        join(s1 - 1, s2 - 1, father)
    for i in range(n):
        if father[i] == i:
            count += 1
    case_num += 1
    print(f"Case {case_num}: {count}")
```



代码运行截图







## 2. 学习总结和收获

BinHeap 与 AVL tree好难





