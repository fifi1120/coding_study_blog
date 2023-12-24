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
        return self.items.pop()  # 使用 pop 方法来移除最后一个元素

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
