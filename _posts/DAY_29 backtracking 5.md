### 491.递增子序列

这道题介绍了去重的新办法。

去重的2个选择：

1. 先sort nums，并在for loop里用if i>startIndex
2. 在for loop之前用一个set记录已经使用过的item，在for loop用if nums[i] in usedSet
   
```
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.backtracking(nums, 0, [], res)
        return res

    def backtracking(self, nums, startIndex, path, res):
        if len(path) >= 2:
            res.append(path[:])
        usedSet = set()
        for i in range(startIndex, len(nums)):
            if (nums[i] in usedSet) or (path and nums[i] < path[-1]):
                continue
            path.append(nums[i])
            usedSet.add(nums[i]) # list的添加用append，set的添加用add
            self.backtracking(nums, i+1, path, res)
            path.pop()
```

### 46.全排列
组合问题，并不需要用到startIndex了。就直接看”用过“与否。
```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.backtracking(nums, [], res)
        return res

    def backtracking(self, nums, path, res):
        if len(path) == len(nums):
            res.append(path[:]) #这里千万别漏了[:]
            return
        for i in range(0, len(nums)):
            if nums[i] in path:
                continue
            else:
                path.append(nums[i]) # 这里千万别漏了append
            self.backtracking(nums, path, res)
            path.pop()
```
