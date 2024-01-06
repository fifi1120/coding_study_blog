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
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        return self.height(root) != -999


    def height(self, root):
        if not root:
            return 0
        left = self.height(root.left)
        if left == -999:
            return -999
        
        right = self.height(root.right)
        if right == -999:
            return -999

        gap = abs(left - right)
        if gap > 1:
            return -999

        return max(left, right)+1
# 如果上面的几种情况都没有发生（一直看起来都是平衡的），没有提前结束循环，就来到这一步：计算新的高度。
# 这里return树的高度需要时准确的数值而不是True / False就行，因为在递归的时候，需要用这个数值不停计算abs(left-right)。
```

这个的时间复杂度是O(n)，因为只遍历了一遍。空间复杂度还是O(n)，即最坏情况下树完全不平衡，像是链表，需要调用栈的深度是O(n)。

### 257. 二叉树的所有路径

这道题会采用前序遍历（中左右），因为是要不断回溯的，先装进去中间节点，后面的左右进行反复地装进去/弹出来。

今天学到的新知识：C++里面的vector对应python是list，push_back是append,pop_back是pop().

这道题设计到回溯backtracking，因为不能除了递归，这道题还要求回到上一步，然后接着重新递归。

请注意：回溯和递归是一一对应的，有一个递归，就要有一个回溯，回溯要和递归永远在一起，不可以把递归和回溯拆开。

```
class Solution:
    def binaryTreePaths(self, root): #这是题目要求的
        if not root:
            return []
        result = []
        path = []
        self.traversal(root, path, result) # 初始状态path是每一个每一个的路径list，res是对path进行变string操作后，把所有的path装到一起
        return result
        
    def traversal(self, root, path, result): # 这里的root在每一次递归的时候都会变
        path.append(root.val)  # 中-->前序遍历先装进去中间的，千万不要漏掉哈。先写这个，再写终止条件，因为中间节点是一定会被放进去的，无论有没有子树。
        if not root.left and not root.right:  # 终止条件这里不是if not root了，而是：到达叶子结点，那么这一条path就可以结束并且被放进res了
            string_path = '->'.join(str(node) for node in path) # 怎么把list转化成string并用特殊符号链接，需要写熟一点
            result.append(string_path)
            return #一般对于嵌套函数，如果不是一定要在里面的函数得到一个什么结果（如高度），那么直接return停止递归就好了。因为你的path和res都是in-place修改的，直接在外层取用，内层return就好了，简化递归逻辑。同理，像110题，如果你想传达某个信息，你可以用-999这种特殊值简化，用嵌套true or false很容易混乱。【对于处理累积结果（如路径列表）的递归函数，通常不需要在内部函数中返回累积结果。】
# 这个 return 停止的是对当前节点（在这种情况下是一个叶子节点）的 traversal 函数调用。它并不会影响到其他路径的遍历或其他递归调用。换句话说，它只结束当前路径的处理。
        if root.left:  # 左
            self.traversal(root.left, path, result)
            path.pop()  # 回溯，紧接在递归后面
        if root.right:  # 右
            self.traversal(root.right, path, result)
            path.pop()  # 回溯
```
小tip：str(node) for node in path 这句话也可以写成map(str, path) 这是map函数的用法，map(某function, 某list) 表示for item in list, 对item对function操作。

### 404.左叶子之和
```
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        lst = []
        self.find_leftleaf(root, lst)
        return sum(lst)

    def find_leftleaf(self, root, lst):
        if not root: # base case
            return 0
        if root and root.left and not root.left.left and not root.left.right: # 只有在上一层的时候，才能更好地判断下一层是不是左叶子
            lst.append(root.left.val) # 请注意这后面千万没有跟着return，这就是一个满足条件就加进去的，很简单的思路不用复杂化了
        if root.left:
            self.find_leftleaf(root.left, lst)
        if root.right:
            self.find_leftleaf(root.right, lst)
```

不用嵌套函数的方法（左右中，后序）：
```
class Solution:
    def sumOfLeftLeaves(self, root):
        if root is None:
            return 0
        if root.left is None and root.right is None:
            return 0
        
        leftValue = self.sumOfLeftLeaves(root.left)  # 左
        if root.left and not root.left.left and not root.left.right:  # 左子树是左叶子的情况
            leftValue = root.left.val
            
        rightValue = self.sumOfLeftLeaves(root.right)  # 右

        sum_val = leftValue + rightValue  # 中
        return sum_val
```

#### 一个很重要的问题：
为什么257. 二叉树的所有路径的base case是if not root.left and not root.right:

但是404.左叶子之和的base case只是简单的if not root:，而不是if root and root.left and not root.left.left and not root.left.right:，

这是因为两道题的要求不一样，所以实现的方式也有所不同。

257要求求很多不同的path，所以必然得不停地在达到叶子节点的时候return，得到一条一条的path，所以终止条件是需要在path结束的时候终止；但是404只需要把所有的节点全部遍历，过程中把某些满足条件的节点加到某个list里面就好了，只有从头到尾遍历一遍，那终止条件当然是if not root。
