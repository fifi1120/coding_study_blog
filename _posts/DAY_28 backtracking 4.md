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
