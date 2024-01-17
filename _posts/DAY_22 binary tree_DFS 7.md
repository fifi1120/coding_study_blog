### 701.二叉搜索树中的插入操作

直接插入到叶子节点是最简单的。

自己的做法：
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
            else: # root没有.right了，没有右子树了，就是已经到叶子节点了，就把现在的“root”和node连起来。
                root.right = node

        if val < root.val:
            if root.left:
                self.insertIntoBST(root.left, val)
            else:
                root.left = node

        return root # 当然要return root啦，你最后是要返回一个新的树呀
```
卡哥的做法：
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
        if not root: # 会一直到这一步的，就是到叶子节点的时候，会return我要插入的节点，把这个目标节点传到上一层
            return node 
        
        if val > root.val:
            root.right = self.insertIntoBST(root.right, val) # 等到了叶子节点那里的时候，相当是 某叶子节点.right = node

        if val < root.val:
            root.left = self.insertIntoBST(root.left, val)

        return root # 当然要return root啦，你最后是要返回一个新的树呀
```
