### 654.最大二叉树

庆祝一下第一次自己写出tree的medium题目，其实和105、106非常相似

```
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        
        root_val = max(nums)
        root = TreeNode(root_val)

        root_index = nums.index(root_val)

        prefix = nums[:root_index]
        suffix = nums[root_index + 1:]

        root.left = self.constructMaximumBinaryTree(prefix)
        root.right = self.constructMaximumBinaryTree(suffix)

        return root
```
### 617.合并二叉树 

庆祝自己第二次独立写出tree的题目

```
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1 and not root2:
            return None
        if not root1:
            return root2
        if not root2:
            return root1
        root_val = root1.val + root2.val
        root = TreeNode(root_val)

        root.left = self.mergeTrees(root1.left, root2.left)
        root.right = self.mergeTrees(root1.right, root2.right)

        return root
```
### 700.二叉搜索树中的搜索 

庆祝自己第三次独立写出tree的题目。。。。。。。
自己的写法：
```
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return None
        if root.val == val:
            return root
        if root.val > val:
            return self.searchBST(root.left, val)
        else:
            return self.searchBST(root.right, val)
        # return None - 这里其实可以不写最后一句，因为反正总会递归到if not root: return None的那一步的
```
其实用迭代法（While loop)也是非常直觉性和方便的：
```
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        while root:
            if val > root.val:
                root = root.right
            elif val < root.val:
                root = root.left
            else:
                return root
```

### 98.验证二叉搜索树

二叉搜索树的陷阱：需要左子树<中间节点<右子树，而不能仅仅是左节点<中间节点<右节点。

其实二叉搜索树有个特点，就是如果你中序遍历（左中右）并放进list，会发现list里面的数字是从小到大排列的 - 可以用这个特性去做。

```
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        self.lst = [] # 这里生命全局变量之后，记得以后出现lst都要加self了。
        self.traversal(root)
        for i in range(1, len(self.lst)):
            if self.lst[i] <= self.lst[i-1]:
                return False
        return True


    def traversal(self, root):
        if not root:
            return
        self.traversal(root.left)
        self.lst.append(root.val)
        self.traversal(root.right)
```
