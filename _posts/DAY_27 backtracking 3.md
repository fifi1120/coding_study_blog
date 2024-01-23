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
