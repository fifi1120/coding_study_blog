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

### 450.删除二叉搜索树中的节点

如果要删除的节点在root右边--> root.right = return 出来的新东西 接住（会在root.val == key的那一层return的）

如果要删除的节点在root左边--> root.left = return 出来的新东西 接住（会在root.val == key的那一层return的）

5种情况：
1. 没找到要删除的节点（直接return，不用额外操作，遍历完到叶子节点了，就停止recursion了）
2. 删的是叶子节点（return None到上一层）
3. 目标节点左子树不是空的，右子树是空的（return 右孩子 到上一层）
4. 目标节点右子树不是空的，左子树是空的（return 左孩子 到上一层）
5. 目标节点左右子树都有内容（让右孩子上位，把原来的左孩子链接在右孩子的最左下角，return 新的右孩子）



   
```
class Solution:
    def deleteNode(self, root, key):
        if root is None: # 情况一：到叶子了，所以return直接停止，也不改变什么了，没有要接住什么的需求
            return # 并不只在情况一发生，如果是情况2-4，也会走到叶子节点
        if root.val == key: 
            if root.left is None and root.right is None: # 情况二：删除的是叶子节点
                return None # 直接删了，把None传到上一层
            elif root.left is None: # 情况三：左孩子是空，右孩子有数
                return root.right 
            elif root.right is None: # 情况四：右孩子是空，左孩子有数
                return root.left
            else: # 情况五：左右孩子都有数
                cur = root.right # 让cur指针先指向右孩子，准备让右孩子上位
                while cur.left is not None: 
                    cur = cur.left # 让cur指向右子树的左边最下角
                cur.left = root.left # 链接【原右子树的左边最下角】.left = 原左子树
                return root.right # 
        if root.val > key:
            root.left = self.deleteNode(root.left, key)
        if root.val < key:
            root.right = self.deleteNode(root.right, key)
        return root # 最后还是return整棵树
```
