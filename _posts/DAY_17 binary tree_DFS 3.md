leetcode一般是规定根节点的深度是1；叶子节点的高度是1.

不管是求高度还是求深度（这两者一般可以互换的），都建议用”求高度“的办法，用后序遍历（左右中）的顺序去求，因为代码最简便（每个根结点可以向左右子树要信息）。

### 110.平衡二叉树

平衡二叉树：一个二叉树 每个节点 的左右两个子树的 高度差的绝对值不超过1。

”每个节点“ ”子树高度“ ---> 这里就很自然地想到 recursion+求高度的helper function。

比较直接（但复杂度较高）的操作是：

```
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        if root is None:
            return True

        leftDepth = self.getDepth(root.left) # 二次递归了
        rightDepth = self.getDepth(root.right)

        if abs(leftDepth - rightDepth) > 1:
            return False

        return self.isBalanced(root.left) and self.isBalanced(root.right)

    def getDepth(self, node: TreeNode) -> int: # 常规求深度的操作
        if node is None:
            return 0

        leftDepth = self.getDepth(node.left)
        rightDepth = self.getDepth(node.right)

        return max(leftDepth, rightDepth) + 1
```
