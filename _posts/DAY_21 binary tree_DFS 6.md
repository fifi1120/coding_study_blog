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



