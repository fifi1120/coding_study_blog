### 530.二叉搜索树的最小绝对差 
自己可以独立做出来，很棒，二刷的时候再去领悟一下双指针。
```
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.lst = []
        self.traversal(root)
        min_diff = max(self.lst) - min(self.lst)
        for i in range(1, len(self.lst)):
            if self.lst[i] - self.lst[i-1] < min_diff:
                min_diff = self.lst[i] - self.lst[i-1]
        return min_diff


    def traversal(self, root):
        if not root:
            return
        self.traversal(root.left)
        self.lst.append(root.val)
        self.traversal(root.right)
```

###  501.二叉搜索树中的众数 

大致思路自己知道，但是collections.Counter函数和字典操作还是不太熟练。可以再做。

二刷再领悟双指针

```
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        self.lst = []
        self.traversal(root)
        left = 0
        count_map = collections.Counter(self.lst)
        max_count = max(count_map.values()) # 不熟练 关注
        return [num for num, count in count_map.items() if count == max_count] # 不熟练 关注
 
    def traversal(self, root):
        if not root:
            return
        self.traversal(root.left)
        self.lst.append(root.val)
        self.traversal(root.right)
```

## python字典由value查key的三种方法：

<img width="683" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/45eaf9a8-26db-4f4a-9027-e5d9eb9df39a">

<img width="494" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/f09cebdb-b088-436f-aa57-cdf565d48d6e">


### 236. 二叉树的最近公共祖先

首先，是要找祖先——一般树都是从上到下去遍历的，但是如果你要从下到上，唯一办法就是回溯，而后序遍历（左右中）就是最自然的回溯（把左右子树的信息传递到上一层的中间节点）。

我们的思路是：如果找到一个节点，发现左子树出现结点p，右子树出现节点q，或者 左子树出现结点q，右子树出现节点p，那么该节点就是节点p和q的最近公共祖先。

```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root == q or root == p or root is None: 
# base case：1.如果到叶子节点不能再往下了，即root is None 2.找到了p或者找到了q，就return 当前的这个p或者q
            return root

        left = self.lowestCommonAncestor(root.left, p, q) # 左 。 left_subtree就是往左边遍历的最早找到的含p/q的那颗树（如果走到最左下还没有的话，还是会(if root is None: return root-None) 此时就会到下一句话right_subtree = self.lowestCommonAncestor(root.right, p, q) 往右边去看，其实就是后序遍历的基础做法左右中。到最后所有节点都会被遍历一遍的。
        right = self.lowestCommonAncestor(root.right, p, q) # 右 。 right_subtree就是往右边遍历的最早找到的含p/q的那颗树

        if left is not None and right is not None: 
# 处理中间节点ing - 最关键的一步是：如果左边有返回值（就是找到p了，返回了p）+右边有返回值（就是找到q了，返回了q），证明此时的root就是左边和右边上面的那个节点
            return root

        if left is None and right is not None:
            return right
        elif left is not None and right is None:
            return left
        else: 
            return None

# 如果是example 2这种情况，其实并不需要在代码中额外做一些补充，因为直接覆盖在目前的算法中了（如果left和right其中只有一个有返回值，那么就会返回有值的那个；所以最终到最顶部，反正总是会传入更高的那个p/q的返回值的。--- 如果找不到p/q返回的是None，当一个是None一个是真正的值，最终会返回真正值。
```
