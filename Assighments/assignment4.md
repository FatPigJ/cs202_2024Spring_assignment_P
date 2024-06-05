# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

操作系统：Windows 11

Python编程环境：Pycharm



## 1. 题目

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/



思路：deque



代码

```python
from collections import deque

t = int(input())
for test_case in range(t):
    queue = deque()
    n = int(input())

    for operation in range(n):
        type, value = map(int, input().split())
        if type == 1:
            queue.append(value)
        else:
            if value:
                queue.pop()
            else:
                queue.popleft()

    if queue:
        print(*queue)
    else:
        print("NULL")
```



代码运行截图 







### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：自己维护栈，使用eval()



代码

```python
stack = []
for item in reversed(input().split()):
    if item in '+-*/':
        stack.append(str(eval(stack.pop() + item + stack.pop())))
    else:
        stack.append(item)
print("%.6f"%float(stack[0]))
```



代码运行截图 





### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：shunting yard



代码

```python
n = int(input())
precedence = {'*':2, "/":2, '+':1, '-':1}

for test_case in range(n):
    expr = input()
    stack = []
    postfix = []
    num = ''

    for char in expr:
        if char.isnumeric() or char == '.':
            num += char
        else:
            if num:
                stack.append(num)
            num = ''

            if char == '(':
                postfix.append(char)
            elif char == ')':
                while postfix[-1] != '(':
                    stack.append(postfix.pop())
                postfix.pop()
            elif char in '+-*/':
                while postfix and (postfix[-1] != '(' and precedence[postfix[-1]] >= precedence[char]):
                    stack.append(postfix.pop())
                postfix.append(char)

    if num:
        stack.append(num)

    while postfix:
            stack.append(postfix.pop())

    print(*stack)
```



代码运行截图







### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：

如注释

代码

```python
from collections import deque
string = input()

#模拟原始出栈过程
while 1:
    try:
        sample = input()
        queue = deque(list(string)) #sample中的字符
        stack = [] #模拟栈
        flag = 1
        
        if len(sample) != len(string):
            print('NO')
            continue

        for char in sample:
            if stack and char == stack[-1]: #若字符在stack顶，自然原始过程为把此字符出栈
                stack.pop()
            else:
                try:
                    i = queue.index(char) #若字符在未入栈queue中，自然该字符之前的字符必定在该字符在出栈前依次入栈
                    for j in range(i):
                        stack.append(queue.popleft())
                    queue.popleft()
                except ValueError: #若字符在栈中且不在栈底，则不可能提前出栈
                    flag = 0
                    break

        if flag:
            print('YES')
        else:
            print('NO')

    except EOFError:
        break
```



代码运行截图 







### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：使用列表模拟tree



代码

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

n = int(input())
tree = [Node(i) for i in range(n)]
for i in range(n):
    l, r = map(int, input().split())
    tree[i].left = (tree[l-1] if l != -1 else None)
    tree[i].right = (tree[r-1] if r != -1 else None)

possible_root = [1 for i in range(n)]
for node in tree:
    if node.left:
        possible_root[node.left.value] = 0
    if node.right:
        possible_root[node.right.value] = 0
for i in range(n):
    if possible_root[i]:
        root = i
        break

def depth(node):
    if node == None:
        return 0
    else:
        return max(depth(node.right), depth(node.left)) + 1

print(depth(tree[root]))
```



代码运行截图 







### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：使用mergesort



代码

```python
count = 0
def merge(arr):
    global count
    if len(arr) > 1:
        mid = len(arr) // 2
        L = arr[:mid]
        R = arr[mid:]
        merge(L)
        merge(R)

        i,j,k = 0,0,0
        l = mid
        while i<mid and j<len(arr)-mid:
            if L[i] <= R[j]:
                arr[k] = L[i]
                i += 1
                k += 1
                l -= 1
            else:
                arr[k] = R[j]
                j += 1
                k += 1
                count += l
        while i<mid:
            arr[k] = L[i]
            i += 1
            k += 1
        while j<len(arr)-mid:
            arr[k] = R[j]
            j += 1
            k += 1



while 1:
    n = int(input())
    if n == 0:
        break
    seq = []
    for i in range(n):
        seq.append(int(input()))

    count = 0
    merge(seq)
    print(count)
```



代码运行截图







## 2. 学习总结和收获

要注意坑人的数据与条件，如合法出栈中输入数据可能长度不对





