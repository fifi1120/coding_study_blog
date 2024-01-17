### 701.二叉搜索树中的插入操作

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        node = TreeNode(val)
        if not root:
            return node
        
        if val > root.val:
            if root.right:
                self.insertIntoBST(root.right, val)
            else:
                root.right = node

        if val < root.val:
            if root.left:
                self.insertIntoBST(root.left, val)
            else:
                root.left = node

        return root # 当然要return root啦，你最后是要返回一个新的树呀
```
