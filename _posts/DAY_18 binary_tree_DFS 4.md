### 513.找树左下角的值
用BFS deque迭代模板更简单，但是也可以用DFS。

#### BFS：
```
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        if not root:
            return []
        queue = collections.deque([root])
        while queue:
            level = []
            for _ in range(len(queue)):
                cur = queue.popleft()
                level.append(cur.val)
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
        return level[0]
```

#### DFS:
深度最大的叶子节点，就一定是在最后一行的-->题目就是要找最后的一行的最左边节点。

遍历顺序：只要优先遍历左节点就好了（所以前中后序都可以），因为并不会对中间节点有什么特殊操作。我们这里就使用前序-中左右。

终止条件：叶子节点+此刻的depth>记录的最大深度（打擂台）

但其实这里有回溯的，因为这里要算高度的话，先算到中节点高度1，再去看左子树，高度+1=2，此时如果要去到右节点，就是高度-1（先回到中节点的高度），再root.right，然后高度+1=2，表示右子树节点是2。

```
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        self.max_depth = -999
        self.res = None
#请注意：这里的max_depth要加self。要加self的有两种情况：1. init里面的 2. 在同一个class里面，但是别的函数traversal调用了这个函数里面的变量。
        self.traversal(root, 0)
        return self.res

    def traversal(self, root, depth):
        if not root.left and not root.right:  # base case就是当此节点是叶子节点
            if depth > self.max_depth:
                self.max_depth = depth
                self.res = root.val
            return
# return结束的是当前的递归调用。这里发生的事情实际是：我本来是在一直向左下角走，直到走到叶子节点；如果是叶子节点（也就是达到base case），我就结束当前的递归调用，然后去到右边节点（由于操作的方式是用root.left root.left，还是通过root去reach到左右节点的，所以期间会做回溯保证高度仍然正确）；每次reach到叶子节点的时候，我都会看是否已经是max深度了，如果是，我就更新全局变量result。所以这里的return其实是停止了每次找叶子节点的过程，而不是直接停止了下面一整块traversal函数，我直接不停更新result就好了。尽量让return处理小一点的逻辑。
# 只要树没有被遍历完，即使return了，return的也只是当前reach叶子节点的这条路，之后还会通过node.left node.right去遍历完整棵树。

        if root.left:
            depth += 1
            self.traversal(root.left, depth)
            depth -= 1
        if root.right:
            depth += 1
            self.traversal(root.right, depth)
            depth -= 1
```

### 112. 路径总和

和257. 二叉树的全部路径很类似

```
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        self.targetSum = targetSum # 这句话很重要，因为targetSum即将被同一个class下的不同函数所用到
        self.res = False # res也要加self，因为res即将被同一个class下的不同函数所用到。
# res要先被设置成False，因为如果满足条件会在下面函数被调成True。我一开始是把res设置成None，在traversal里如果满足targetSum就是True，否则就是False。这样不对，因为traversal函数的return是return了当前的path，但是因为res是全局变量，别的path如果不满足targetSum，又会把res调成False，但题目要求只要有一条路径是满足targetSum就好了，所以和题意不相符。
        path = []
        self.traversal(root, path)
        return self.res

    def traversal(self, root, path):
        path.append(root.val)
        if not root.left and not root.right:
            if sum(path) == self.targetSum:
                self.res = True 
                return
        if root.left:
            self.traversal(root.left, path)
            path.pop()
        if root.right:
            self.traversal(root.right, path)
            path.pop()
```
### 类似题目：113. 路径总和ii
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        if not root:
            return []
        res = []
        path = []
        self.targetSum = targetSum
        self.traversal(root, path, res)
        return res

    def traversal(self, root, path, res):
        path.append(root.val)
        if not root.left and not root.right:
            if sum(path) == self.targetSum:
                res.append(list(path)) # 请注意这里一定是list(path)，而不能简单的res.append(path)
# 如果直接res.append(path)，会出现：当一个路径被添加到 res 中时，它是作为一个引用被添加的。这意味着，后续对 path 的任何修改都会影响到已经存储在 res 中的那个路径。为了修复这个问题，我们需要在添加路径到 res 时创建 path 的一个副本。
# 在 Python 中，当您使用 list(path) 时，您实际上是创建了 path 的一个浅拷贝（shallow copy）。这意味着您创建了一个新的列表对象，其内容是原始列表 path 中元素的副本。
                return
        if root.left:
            self.traversal(root.left, path, res)
            path.pop()
        if root.right:
            self.traversal(root.right, path, res)
            path.pop()
```
