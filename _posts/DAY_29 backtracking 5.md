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
