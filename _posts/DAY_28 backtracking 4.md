### 93.复原IP地址
```
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        results = []
        self.backtracking(s, 0, [], results)
        return results

    def backtracking(self, s, startIndex, path, results): # path还是list，但是装进res的时候是string很合理
        if startIndex > len(s)-1 and len(path) == 4: # path的元素够到装盘的2个条件都得满足才行
            results.append('.'.join(path))
            return

        if len(path) > 4:  # 剪枝 对于时间复杂度提升挺大的，别漏了
            return

        for i in range(startIndex, len(s)):
            if self.is_valid(s[startIndex:i+1]):
                path.append(s[startIndex:i+1])
                self.backtracking(s, i+1, path, results)
                path.pop()

    def is_valid(self, string):
        if string[0] == '0' and len(string) != 1:  # 0开头，但又不是只是0的数字不合法。特别注意：这里是if string[0] == "0" 而不是 if string[0] == 0数字0，这里特别容易忽略！！！
            return False
        if int(string) > 255:
            return False
        return True
```

### 78.子集
```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.backtracking(nums, 0, [], res)
        return res

    def backtracking(self, nums, startIndex, path, res):
        res.append(path[:]) # 我之前觉得这里很难写base case：到底什么时候停呢？但其实，我们会发现，之前的题目都是到叶子节点后把path装进res，但是这里是收集子集，你会发现每一个节点，都会成为我的答案。所以这里并不需要像以前一样，if 到达叶子节点: res.append(path[:])而是每次递归后（下一层后）直接res.append(path[:])，就装进了所有的节点。
        if startIndex > len(nums) - 1:
            return # 所谓的base case是这样的，其实不用写，因为下面的for规定了范围。为了模板标准，还是先写上。

        for i in range(startIndex, len(nums)):
            path.append(nums[i])
            self.backtracking(nums, i+1, path, res)
            path.pop()
```

