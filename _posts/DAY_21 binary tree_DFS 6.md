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

