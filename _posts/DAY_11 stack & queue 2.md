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
