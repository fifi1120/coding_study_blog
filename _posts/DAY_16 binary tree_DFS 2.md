### 104.二叉树的最大深度

求高度（从下往上计数，叶子节点是1）用后序遍历（左右中，中间节点最后处理+1）

求深度（从上往下计数，根是1）用前序遍历（中左右，往下找到孩子后处理+1）

<img width="258" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/21294bcd-fcaf-4493-b9e8-183dc6fd1fcc">

根节点的高度就是二叉树的最大深度，所以本题中我们通过后序求的根节点高度来求的二叉树最大深度。

为什么用后序呢？因为中间节点的高度=max(left, right)+1，由底部推导到上部是最直接的，先求left，再求right，最终return中间节点的height。
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root: # base case：等递归到最下层的None了，就是not root了，高度为0.
            return 0
        left = self.maxDepth(root.left) #左
        right = self.maxDepth(root.right) #右
        depth = max(left, right) + 1 #中
        return depth
```
