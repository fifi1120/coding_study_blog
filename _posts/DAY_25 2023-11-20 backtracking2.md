回溯算法的精髓是，用recursion去代替n层for循环。

Leetcode 216 组合总和III 216 Combination Sum III
link: https://leetcode.com/problems/combination-sum-iii/description/

Find all valid combinations of k numbers that sum up to n such that the following conditions are true:

Only numbers 1 through 9 are used.
Each number is used at most once.
Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

思路：

path一维，res二维。

参数：targetSum, k, n, currentSum-当前层的和, StartIndex-从1开始

终止条件：
```
if len(path) == k: #先看卡槽满了
  if targetSum == currentSum: #再看是否满足和
    res.append(path[:])
    return
```


单层搜索的逻辑:




我自己的写法：
```
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        self.backtracking(k, n, [], 1, res)
        return res

    def backtracking(self, k, n, path, startIndex, res):
        if sum(path) == n and len(path) == k:
            res.append(path[:])
# 注意这里一定是append(path[:])，而不是append(path)
            return

        for i in range(startIndex,10):
            path.append(i)
            self.backtracking(k, n, path, i+1, res)
            path.pop()
```

res.append(path): 当你执行这个语句时，你实际上是将 path 列表的引用（或指针）添加到 res 列表中。这意味着 res 中存储的是对同一个 path 列表的引用。所以，当你在后续的回溯过程中修改 path（例如，通过 path.pop()），这些修改也会反映在 res 中存储的那些列表上，因为它们实际上是指向同一个列表的引用。

在 Python 中，并不是所有数据操作都是基于引用的。理解何时是引用（reference-based）操作，何时是值（value-based）操作，对于编写有效和正确的代码很重要。这里有一些关键点：

不可变类型 (Immutable Types):

对于不可变类型，如整数（int）、浮点数（float）、字符串（str）、和元组（tuple），Python 在赋值时实际上是在传递值。当你将一个不可变对象赋给另一个变量时，Python 会创建该对象的一个新副本。
例如，如果你有 a = 5，然后你写 b = a，b 是 5 的一个新副本，而不是指向同一个内存位置的引用。对 b 的任何修改都不会影响 a。


可变类型 (Mutable Types):

对于可变类型，如列表（list）、字典（dict）、和集合（set），Python 使用引用。这意味着当你将一个列表赋给另一个变量时，两个变量都指向内存中的同一个列表。
例如，如果你有 a = [1, 2, 3]，然后你写 b = a，b 是对同一个列表 [1, 2, 3] 的引用。对 b 的任何修改（如添加或删除元素）都会影响 a，因为 a 和 b 指向的是同一个列表。





