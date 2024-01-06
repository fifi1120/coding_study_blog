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
