### 20. 有效的括号
匹配题用栈真的很合适
```
class Solution:
    def isValid(self, s: str) -> bool:
        my_stack = Stack()
        for i in s:
            if (i == ")" and my_stack.peek() == "(") or (i == "}" and my_stack.peek() == "{") or (i == "]" and my_stack.peek() == "["):
                my_stack.pop()
            else:
                my_stack.push(i)
        if my_stack.is_empty(): #如果改成if not my_stack:就不行，因为类的实例不能这样直接判定布尔值。不起作用。
            return True
        else:
            return False

class Stack: # 还需要继续熟练！！！要形成肌肉记忆
    def __init__(self):
        self.lst = []
    
    def peek(self):
        if self.lst:
            return self.lst[-1]

    def pop(self):
        return self.lst.pop()
    
    def push(self, x):
        self.lst.append(x)

    def is_empty(self):
        return len(self.lst) == 0
```

### 1047. 删除字符串中的所有相邻重复项

匹配问题都是栈的强项

要知道栈为什么适合做这种类似于爱消除的操作，因为栈帮助我们记录了 遍历数组当前元素时候，前一个元素是什么。

```
class Solution:
    def removeDuplicates(self, s: str) -> str:
        my_stack = Stack()
        for i in s:
            if not my_stack.is_empty() and i == my_stack.peek():
                my_stack.pop()
            else:
                my_stack.push(i)
        if my_stack.is_empty():
            return ''
        else:
            return ''.join(my_stack.lst)    #lst -> string ，这里不能忘记.lst、、、 实例不能直接进行操作，要加上.lst才是列表。


class Stack:
    def __init__(self):
        self.lst = []

    def pop(self):
        return self.lst.pop()

    def push(self, x):
        self.lst.append(x)

    def peek(self):
        return self.lst[-1]

    def is_empty(self):
        return len(self.lst) == 0
```
如果不能用栈，还可以用双指针模拟栈：
```
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = list(s)
        slow = fast = 0
        length = len(res)

        while fast < length: # 我维护的一直都是slow这个列表
            res[slow] = res[fast] 
            if slow > 0 and res[slow] == res[slow - 1]:
                slow -= 1 #如果重复，就回退slow指针（回退到的那个格子，会在下一轮更新fast指向的新的数字）
            else:
                slow += 1
            fast += 1 #fast就是一直往前走
            
        return ''.join(res[0: slow])
```

###  150. 逆波兰表达式求值 

The division between two integers always truncates toward zero. 这句话的意思是向0取整，比如5除以2应该是2，-5除以2应该是-2。这样就好：res = int(n2/n1)。

但是不能res = n2 // n1，因为这个是向下取整，有负数的时候会出问题，比如会导致5//2=2; -5//2=-3

```
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        my_stack = Stack()
        for i in tokens:
            if not i in "+-*/":
                my_stack.push(int(i))
            else:
                n1 = my_stack.pop()
                n2 = my_stack.pop()
                if i == "+":
                    res = n1 + n2
                elif i == "-":
                    res = n2 - n1
                elif i == "*":
                    res = n2 * n1
                else:
                    res = int(n2/n1)
                my_stack.push(res) 
        return my_stack.lst[0]

class Stack:
    def __init__(self):
        self.lst = []

    def peek(self):
        return self.lst[-1]
    
    def push(self, x):
        self.lst.append(x)

    def pop(self):
        return self.lst.pop()

    def is_empty(self):
        return len(self.lst) == 0
```
时间空间复杂度都是O(N)。
