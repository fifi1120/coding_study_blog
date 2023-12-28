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
        if not root: #这里必须写，因为下面会有cur.val，如果根本就没有cur就会报错的。
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
