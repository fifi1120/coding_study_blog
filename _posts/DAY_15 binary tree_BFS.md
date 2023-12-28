队列先进先出，适合一层一层（离根从近到远）遍历，也就是BFS。

# 队列迭代法（常见！优于回溯法）

```
# 利用长度法
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root: #必须一开头就写，不然后面操作会报错。
            return []
        queue = collections.deque([root]) #deque后面的括号里面要填入一个列表，这样才能通过这句话把列表转化成deque
        result = []
        while queue: #当队列有数字的时候，考虑 1.把这些数字依次弹出并存进level 2.把这些数字的左右子节点依次存进que。
            #同期在队列中的就是同一层的。
            level = [] #level里面会装这一层的节点有哪些
            for _ in range(len(queue)): #队列里面有几个值就loop几遍（每loop一次，都：1. 弹出那个值 2.把那个值的左子节点和右子节点装进队列）
                cur = queue.popleft()
                level.append(cur.val) #别忘了.val
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            result.append(level)
        return result #结果会是类似这样：[[1], [2, 3], [4, 5]] 大列表套小列表，小列表装的就是那一层的节点
```

上面这个模板，做以下10道题不是问题。

小窍门：基本上很多都是可以先写出模板，再对res进行操作，不需要在模板内部更改。
```
102.二叉树的层序遍历
107.二叉树的层次遍历II
199.二叉树的右视图
637.二叉树的层平均值
429.N叉树的层序遍历：对children的操作比较特殊，值得再做
515.在每个树行中找最大值
116.填充每个节点的下一个右侧节点指针：关注树中链表，值得再做
117.填充每个节点的下一个右侧节点指针II
104.二叉树的最大深度
111.二叉树的最小深度：加了一点有意思的小变化
```
### 429.N叉树的层序遍历
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return []
        res = []
        que = collections.deque([root])
        while que:
            level = []
            for i in range(len(que)):
                cur = que.popleft()
                level.append(cur.val)
                if cur.children:
                    for child in cur.children:
                        que.append(child)
            res.append(level)
        return res
```

### 116.填充每个节点的下一个右侧节点指针
```
class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if not root:
            return root # 注意是写成return root而不是return []，因为要return的是一个节点

        que = collections.deque([root])
        while que:
            level_size = len(que)  #把当前的que的size先存下来（因为之后你会不停append进行左右子树，会导致你对“这一行是否操作完了”的困惑
            for i in range(level_size):
                cur = que.popleft()
                if i <= level_size - 2:  #如果当前的i不是最后一个节点，连接cur和队列中的下一个
                    cur.next = que[0]
                # else:
                #     cur.next = None #其实这段是不用写的，因为如果你不操作任何，本身每个节点都会连接None的
                if cur.left:
                    que.append(cur.left)
                if cur.right:
                    que.append(cur.right)
                                
        return root
```
即使在 while 循环中嵌套了 for 循环，整体的时间复杂度仍然是 O(n)，其中 n 是树中节点的总数。这是因为：

每个节点仅被遍历和处理一次。
for 循环的迭代次数是基于队列的当前长度（即当前层的节点数），而每个节点只会进入队列一次。
while 循环会持续直到队列为空，这确保了每个节点都被访问。
因此，尽管代码中有两层循环，每个节点依然只被处理一次，使得总的操作次数与节点数成线性关系。

### 111.二叉树的最小深度
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        res = 1
        que = collections.deque([root])
        while que:
            for i in range(len(que)):
                cur = que.popleft()
                if cur.left:
                    que.append(cur.left)
                if cur.right:
                    que.append(cur.right)
                if not cur.left and not cur.right:
                    return res
            res += 1
        return res
```
