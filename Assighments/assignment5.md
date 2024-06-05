# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：PyCharm





## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：按描述思路



代码

```python
class Node(object):
    def __init__(self):
        self.left = None
        self.right = None

def height(root: Node):
    if root == None:
        return 0
    if root.left == None and root.right == None:
        return 0
    return max(height(root.left) + 1, height(root.right) + 1)

def count_leaves(root: Node):
    if root == None:
        return 0
    if root.left == None and root.right == None:
        return 1
    return count_leaves(root.left) + count_leaves(root.right)

n = int(input())
fltr = [1 for i in range(n)]
tree = [Node() for i in range(n)]
for i in range(n):
    l, r = map(int, input().split())
    if l != -1:
        fltr[l] = 0
        tree[i].left = tree[l]
    if r != -1:
        fltr[r] = 0
        tree[i].right = tree[r]

for i,v in enumerate(fltr):
    if v:
        root = i
        break

print(height(tree[root]), count_leaves(tree[root]))
```



代码运行截图 







### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：自己维护stack，参照讲义思路



代码

```python
class Node(object):
    def __init__(self):
        self.value = None
        self.children = []

def preorder(node: Node):
    return node.value + ''.join(map(preorder, node.children))

def postorder(node: Node):
    return ''.join(map(postorder, node.children)) + node.value

string = input()
stack = []
curr = Node()
for char in string:
    if char.isalpha():
        curr.value = char
    elif char == '(':
        stack.append(curr)
        curr = Node()
        stack[-1].children.append(curr)
    elif char == ')':
        curr = stack.pop()
    elif char == ',':
        curr = Node()
        stack[-1].children.append(curr)
root = curr

print(preorder(root))
print(postorder(root))
```



代码运行截图 







### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：使用树结构，用stack parse tree



代码

```python
class Folder(object):
    def __init__(self, name=None):
        self.name = name
        self.sub = []
        self.files = []

gap = '|     '

def read(root: Folder, lev):
    return ''.join([gap*lev + root.name + '\n'] + [''.join([read(i, lev+1) for i in root.sub])] + [''.join([gap*lev + i + '\n' for i in sorted(root.files)])])

stack, curr = [], Folder()
def oper(inp):
    global stack
    global curr
    if inp[0] == 'f':
        curr.files.append(inp)
    elif inp[0] == 'd':
        stack.append(curr)
        curr = Folder(inp)
        stack[-1].sub.append(curr)
    elif inp == ']':
        curr = stack.pop()

output = []
data_set_no = 1
while 1:
    curr = Folder('ROOT')
    stack = []
    inp = input()
    if inp == '#':
        output.pop()
        print(*output, sep='\n')
        break
    elif inp == '*':
        output.append(f'DATA SET {data_set_no}:')
        output.append('ROOT')
        output.append('')
    else:
        oper(inp)
        while 1:
            inp = input()
            if inp == '*':
                output.append(f'DATA SET {data_set_no}:')
                output.append(read(curr, 0))
                output[-1] = output[-1][:-1]
                output.append('')
                data_set_no += 1
                break
            else:
                oper(inp)
```



代码运行截图







### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：从树叶开始解析表达式，用队列输出表达式



代码

```python
from collections import deque
class Node(object):
    def __init__(self, value=None, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

def parse(expr: str):
    stack = []
    for char in expr:
        if char.islower():
            stack.append(Node(char))
        else:
            stack.append(Node(char, stack.pop(), stack.pop()))
    return stack[-1]

def read(root: Node):
    output = deque()
    queue = deque()

    output.appendleft(root.value)
    queue.append(root)
    while queue:
        node = queue.popleft()
        if node.right:
            output.appendleft(node.right.value)
            queue.append(node.right)
        if node.left:
            output.appendleft(node.left.value)
            queue.append(node.left)

    return ''.join(output)

n = int(input())
for i in range(n):
    print(read(parse(input())))
```



代码运行截图







### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：通过判断root在中序表达式位置，区分左树和右树，然后递归实现



代码

```python

class Node(object):
    def __init__(self, value=None, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

    def __str__(self):
        return self.value

    def __bool__(self):
        return True

def parse(inorder: str, postorder: str, root: Node):
    if len(inorder) > 1:
        idx = inorder.find(root.value)

        left = Node(postorder[idx - 1]) if idx>=1 else None
        right = Node(postorder[-2]) if idx<=len(postorder)-2 else None
        root.left = left
        root.right = right

        if left:
            parse(inorder[:idx], postorder[:idx], left)
        if right:
            parse(inorder[idx+1:], postorder[idx:-1], right)

def preorder_traversal(root: Node):
    return root.value + (preorder_traversal(root.left) if root.left else '') + (preorder_traversal(root.right) if root.right else '')

inorder = input()
postorder = input()
root = Node(postorder[-1])
parse(inorder, postorder, root)
print(preorder_traversal(root))
```



代码运行截图 







### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：与上一题类似，改了一些部分



代码

```python

class Node(object):
    def __init__(self, value=None, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

    def __str__(self):
        return self.value

    def __bool__(self):
        return True

def parse(inorder: str, preorder: str, root: Node):
    if len(inorder) > 1:
        idx = inorder.find(root.value)

        left = Node(preorder[1]) if idx>=1 else None
        right = Node(preorder[idx+1]) if idx<=len(preorder)-2 else None
        root.left = left
        root.right = right

        if left:
            parse(inorder[:idx], preorder[1:idx+1], left)
        if right:
            parse(inorder[idx+1:], preorder[idx+1:], right)

def postorder_traversal(root: Node):
    return (postorder_traversal(root.left) if root.left else '') + (postorder_traversal(root.right) if root.right else '') + root.value

while 1:
    try:
        preorder = input()
        inorder = input()
        root = Node(preorder[0])
        parse(inorder, preorder, root)
        print(postorder_traversal(root))
    except EOFError:
        break
```



代码运行截图







## 2. 学习总结和收获

文件结构图遇到了Presentation Error，发现最后输出多了一个换行，以后要多加注意





