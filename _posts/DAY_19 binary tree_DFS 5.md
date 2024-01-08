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
