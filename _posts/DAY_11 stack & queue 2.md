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

        while fast < length:
            # 如果一样直接换，不一样会把后面的填在slow的位置
            res[slow] = res[fast]
            
            # 如果发现和前一个一样，就退一格指针
            if slow > 0 and res[slow] == res[slow - 1]:
                slow -= 1
            else:
                slow += 1
            fast += 1
            
        return ''.join(res[0: slow])
```
