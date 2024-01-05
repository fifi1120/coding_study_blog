leetcode一般是规定根节点的深度是1；叶子节点的高度是1.

不管是求高度还是求深度（这两者一般可以互换的），都建议用”求高度“的办法，用后序遍历（左右中）的顺序去求，因为代码最简便（每个根结点可以向左右子树要信息）。

### 110.平衡二叉树

平衡二叉树：一个二叉树 每个节点 的左右两个子树的 高度差的绝对值不超过1。

”每个节点“ ”子树高度“ ---> 这里就很自然地想到 recursion+求高度的helper function。

比较直接（但复杂度较高）的操作是：

```
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        if root is None:
            return True

        leftDepth = self.getDepth(root.left) # 二次递归了，使时间复杂度达到O(n^2)。
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
空间复杂度主要由递归调用栈的最大深度决定。在最坏的情况下（树完全不平衡，形似链表），递归调用栈的深度可以达到O(n)，其中n是树的高度。因此，空间复杂度为O(n)。

为了降低上述代码的时间复杂度，您可以将 getDepth 方法调整为在计算深度的同时检查树是否平衡。这样一来，每个节点只需访问一次，从而将时间复杂度降低到 O(n)，其中 n 是树的节点数。

修改后的算法在发现任何不平衡的子树时会立即返回一个特殊的值（例如 -999），来表示该子树不平衡。如果子树是平衡的，则返回其高度。

```
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        return self.checkBalance(root) != -999

    def checkBalance(self, node: TreeNode) -> int:
        if node is None:
            return 0

        left_height = self.checkBalance(node.left)
        if left_height == -999:
            return -999  # 左子树不平衡

        right_height = self.checkBalance(node.right)
        if right_height == -999:
            return -999  # 右子树不平衡

        if abs(left_height - right_height) > 1:
            return -999  # 当前节点的子树不平衡

        return max(left_height, right_height)+1
# 如果上面的几种情况都没有发生（一直看起来都是平衡的），没有提前结束循环，就来到这一步：计算新的高度。
# 这里return树的高度需要时准确的数值而不是True / False就行，因为在递归的时候，需要用这个数值不停计算abs(left-right)。
```

这个的时间复杂度是O(n)，因为只遍历了一遍。空间复杂度还是O(n)，即最坏情况下树完全不平衡，像是链表，需要调用栈的深度是O(n)。
