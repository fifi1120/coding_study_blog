# 栈（Stack）的实现
在LeetCode中，栈通常用于解决如括号匹配、后缀表达式计算、回溯问题等。实现方法包括：

### 【重要】使用列表（List）实现stack：
好处：可以通过索引直接访问/替换元素，复杂度都是O(1).
注意：如果要在栈底进行操作（插入或删除），就得移动所有元素了，复杂度O(n)。
```
class Stack:
    def __init__(self):
        self.items = []  # 初始化一个空列表

    def push(self, item):
        self.items.append(item)  # 使用 append 方法来添加元素

    def pop(self):
        return self.items.pop()  # 使用 pop 方法来移除最后一个元素

    def peek(self):
        return self.items[-1] if self.items else None  # 查看栈顶元素

    def is_empty(self):
        return not self.items  # 检查栈是否为空

    def size(self):
        return len(self.items)  # 获取栈的大小
```

### （不推荐，看看就好）使用collections.deque实现stack：

当需要对栈的底部进行插入或删除操作时，collections.deque会更高效，因为在首尾操作deque的复杂度都是O(1)。

```
class Stack:
    def __init__(self):
        self.items = collections.deque()  # 初始化一个空的 deque

    def push(self, item):
        self.items.append(item)  # 使用 append 方法来添加元素

    def pop(self):
        return self.items.pop()  # 使用 pop 方法来移除最后一个元素，记得pop就是popright()的意思，但是是没有popright()这种写法的

    def peek(self):
        return self.items[-1] if self.items else None  # 查看栈顶元素

    def is_empty(self):
        return not self.items  # 检查栈是否为空

    def size(self):
        return len(self.items)  # 获取栈的大小

    def push_bottom(self, item):
          # 添加元素到栈底
          self.items.appendleft(item)

    def pop_bottom(self):
        # 移除栈底元素
        return self.items.popleft()

    def peek_bottom(self):
            # 查看栈底元素
            return self.items[0] if self.items else None
```

# 队列（Queue）的实现
在LeetCode中，队列常用于广度优先搜索（BFS）、缓存设计等问题。实现方法包括：

### 【重要】使用collections.deque实现queue：

deque是基于Block-linked List的数据结构，提供了快速的从两端添加和弹出元素的操作，非常适合实现队列。
特别是在需要广度优先搜索时，deque是优先选择。

```
class Queue:
    def __init__(self):
        self.items = deque()

    def enqueue(self, item):
        # 在队列尾部添加元素 O(1)
        self.items.append(item)

    def dequeue(self):
        # 从队列头部移除元素 O(1)
        if self.items:
            return self.items.popleft()
        return None

    def peek(self):
        # 查看队列头部的元素 O(1)
        return self.items[0] if self.items else None

    def is_empty(self):
        # 检查队列是否为空
        return not self.items

    def size(self):
        # 获取队列的大小，O(1)
        return len(self.items)
```

### （不推荐）用list实现queue（除非是要求通过索引频繁访问或修改队列中的元素，或者需要在队列中间进行操作）
```
class Queue:
    def __init__(self):
        self.queue = []

    def enqueue(self, item):
        self.queue.append(item)

    def dequeue(self):
        if len(self.queue) < 1:
            return None
        return self.queue.pop(0)

    def size(self):
        return len(self.queue)
```

#### 232.用栈实现队列
虽然是简单题目，但是很考察基本功。推荐写一写。
#基本功：何时用self？
#回答：在一个 class 里面， 如果变量（在 init 里面定义了）或者函数（在 class 里面定义的），在 class 的别的函数里面调用他们的时候就要加。

```
class MyQueue:

    def __init__(self):
        self.stack_in = []
        self.stack_out = []

    def push(self, x: int) -> None:
        self.stack_in.append(x)
        

    def pop(self) -> int: #pop的话只弹出一个就好了，所以如果stack_out不是空的，就直接弹一个出来就好了。如果tack_out是空的，就先把stack_in的数字【全部弹出并append到stack_out】，再弹出stack_out的最上面一个
        if self.empty():
            return None
        if self.stack_out:
            return self.stack_out.pop()
        else:
            while self.stack_in:
                self.stack_out.append(self.stack_in.pop())
        return self.stack_out.pop()


    def peek(self) -> int:
        peek_item = self.pop()
        self.stack_out.append(peek_item) #这里一定是在stack_out去append这个数字，而不能直接self.push(peek_item)。因为push的时候是把元素放进stack_in，但是你pop的时候是从stack_out出来的，你还原当然要还原成和原来一样的，不然就会乱掉。
        return peek_item
        

    def empty(self) -> bool:
        if len(self.stack_in) == 0 and len(self.stack_out) == 0:
            return True
        return False 
```
