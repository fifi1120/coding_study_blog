### 39. 组合总和

```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        self.backtracking(candidates, target, 0, 0, [], res)
        return res

    def backtracking(self, candidates, target, cur_sum, startIndex, path, res):
        if cur_sum > target:
            return 
        if cur_sum == target:
            res.append(path[:]) # 注意这里千万不能漏了啊，我之前就漏了所以错了
            return
        for i in range(startIndex, len(candidates)):
            path.append(candidates[i])
            cur_sum += candidates[i]
            self.backtracking(candidates, target, cur_sum, i, path, res) # 因为可以重复选取数字，所以是i不是i+1。树枝去重。
            path.pop() # 回溯1
            cur_sum -= candidates[i] # 回溯2
```
### 40.组合总和II

这一题和之前的题目不一样的地方在于，给的list（candidate）本身就有重复的元素了，就不能像之前一样紧靠用i或者i+1（树枝去重）就能重了。还得需要一个树层去重（就是在for loop那里做动作）。

```
class Solution:
    def combinationSum2(self, candidates, target):
        result = []
        candidates.sort() # 因为我要比较candidates[i] == candidates[i - 1]，从而剪枝，所以一定要按照顺序排。
        self.backtracking(candidates, target, 0, 0, [], result)
        return result

    def backtracking(self, candidates, target, cur_sum, startIndex, path, result):
        if cur_sum == target:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            if cur_sum + candidates[i] > target:
                return 

            if i > startIndex and candidates[i] == candidates[i - 1]: # 用i>startIndex去重是树层去重，因为i>startIndex就表明了和之前那个是在同一层。如果继续向下递归的话只能是i>startIndex（会经历i+1）。卡哥的另一个方法是：if i > 0 and candidates[i] == candidates[i - 1] and used[i - 1] == False，这里用used其实是一个意思，就是怕candidates[i]已经比candidates[i-1]要第一层了，所以一定要used[i-1]是没有用过的状态（这里不是说真的没有用过，而是在此刻的i的path中没有用过），这样才能剪。如果不用startIndex也不用used，[1,1,6]这种情况就会被剪枝剪走，这就不对了。
                continue

            cur_sum += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, cur_sum, i + 1, path, result)
            cur_sum -= candidates[i]
            path.pop()

```

### 131.分割回文串

```
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res = []
        self.backtracking(s, 0, [], res)
        return res

    def backtracking(self, s, startIndex, path, res):
        if startIndex > len(s)-1: #切到最后一个。这个分割方法一定是valid的，因为我是按照一定会是回文串的方式切割的---只有确认了是回文串，才会来到这一步（所以最后一个小节也是确认过回文串才来到这一步的吗？--是的。一定走过了整个list，确认了分割方式，分完了之后才能走到这个base case的。）所以，既然已经走到了最后一步，path完整了，此刻需要把path装到res里。可能此刻的path=["aa","b"]
# 注意这里一定是> len(s)-1 因为最后一位索引是len(s)-1，所以当我的startIndex > len(s)-1 就是path已经走完全部了，现在是要终止了的意思。
            res.append(path[:])
            return
        
        for i in range(startIndex, len(s)): # 记得这里一定是startIndex不是0哈！！！！！！！！startIndex是切第一刀，i在切了第一刀的基础上，切第二刀
            if self.isPalin(s[startIndex:i+1]): # 是要看path里面的东西是不是回文子串，我自己最开始直接self.isPalin(path)，就错了，应该是s[startIndex:i+1]。如果这个“里面的东西”是回文串，就把它加到我的path里。
                path.append(s[startIndex:i+1])
                self.backtracking(s, i+1 , path, res) # 这里一定是在for loop下面的。因为只有刚刚切的事回文串了，我才能把i+1当做新的startIndex，继续往下切割，不然就直接i++看下一个i能不能切。
                path.pop() # 千万别漏了

    def isPalin(self, s):
        left = 0
        right = len(s)-1
        while left <= right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
        return True
```
