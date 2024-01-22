回溯其实就是穷举解法，把所有可能性过一遍。

有recursion必有回溯

其实是一种暴力解法

几种适用场景：

1. 组合（无顺序）：在1 2 3 4 找出所有长度为2的组合

2. 切割：有多少种切割方法（在某种规则件下）

3. 子集：有多少种子集（在某种条件下）
  
5. 排列（有顺序）：N个数字（按某种规则）然后全排

6. 棋盘：N皇后、数独


# 把回溯法抽象成树形结构：

回溯法的精髓：在集合中递归查找子集。

用for loop横着遍历，用递归竖着遍历。

<img width="743" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/9802f1e3-4ffd-4926-8e07-237f6eb5098f">



N叉树：

树的宽度是集合的大小（for循环遍历）；

树的深度是递归的深度。


### 回溯法的模板：

1. 递归函数（起名叫backtracking）没有返回值。

一般参数比较多：一般先写逻辑，看后面需要什么参数，就补上什么参数。

   
2. 处理

```
if 终止条件：
   （在叶子节点）收集结果放进结果集 #把刚刚放进数组的1 2放进结果集
      return
```

【只有子集问题是在每一个节点去收集结果，其他的问题都是在最下层节点（即叶子结点）收集结果】

3. 单层搜索逻辑：

```
for 集合的每一个元素: #for循环是进行树的横向遍历-本层节点个数
    处理节点 #把1 2放进一个数组
    递归函数 #递归是纵向遍历：在树形图里面一层层往下走
    回溯操作 #撤销处理节点的结果：比如我现在是1 2然后把2弹出去，把3加进去，变成1 3，再把3弹出去，再把4加进去，变成1 4。回溯是有必要的，不然加到后面就是1 2 3 4 。。。
```


backtracking函数模板：
```
def backtracking(参数):
  if 终止条件：（叶子节点）
    存放结果
    return

  for 本层所有节点：（遍历集合的每一个元素）
    处理节点
    backtracking(走过的路径，结果列表)
    回溯（即撤回刚刚的处理结果）
```


### 77. 组合 

#### 不剪枝：

```
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []  # 存放结果集
        self.backtracking(n, k, 1, [], result)
        return result
    def backtracking(self, n, k, startIndex, path, result):
        if len(path) == k: #终止条件是：当path里面存够了需要的树（到达叶子节点）
            result.append(path[:]) #存进res
            return
        for i in range(startIndex, n + 1):  # 组合问题就要这样做，避免重复。哪怕题目要求可以 用相同元素，也要这么写，这么写的目的是让结果不会有[1,2]和[2,1]这样的重复组合。
            path.append(i)  # 处理节点，放进path
            self.backtracking(n, k, i + 1, path, result) #recursion，其实本身再次调用自己，就是走深一层的意思（沿着树枝往下）。比如k=5，那么其实会一遍一遍不停调用4遍，直到走到叶子节点。这里（把i+1）就是为了避免重复的微调。
            path.pop()  # 回溯，撤销处理的节点

```

        #为什么这里要path[:]而不能直接path：是因为list在python中是可以更改的，如果直接写path，代表的是path的引用，会在后序的递归中不停产生变化。

何时要拷贝：

1. 递归中的列表修改：当你在递归函数中修改一个列表，并且这个修改需要在递归的每一层保持独立时，使用列表的拷贝。这通常在你希望保存当前状态作为结果的时候发生，如在组合或排列生成中。

2. 共享列表的问题：如果你直接使用原始列表（如 path），所有对这个列表的更改都会反映在每个递归层级上，因为它们都指向同一个列表。如果你发现你的结果不是预期的独立组合或排列，而是共享了某些状态，这是一个提示你可能需要使用拷贝。

3. 返回到上一个状态：在回溯算法中，当你需要“撤销”之前的操作并返回到上一个状态时，你应该考虑使用列表的拷贝。这确保了当你回到递归的上一层时，状态是未被后续操作改变的。

4. 添加结果到集合：当你想把当前的组合或路径添加到结果集（如 result.append(path)）时，通常你需要添加的是该状态的快照，而不是实际可变对象的引用。这时，使用 path[:]。

# 总模板：
```
def XXX:
  res = []
  self.backtracking(xxx)
  return res

def backtracking(self, xxx):
  if BASELINE:
    res.append()
    return

  for EACH_IN_THIS_LAYER:
    path.append() # path就是装载小结果的容器，满足条件后把好几个path一组一组放进最终的res
    self.backtracking(xxx)
    path.pop()
```


#### 剪枝：主要是在优化backtracking的单层搜索逻辑。
```
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = [] 
        self.backtracking(n, k, 1, [], result)
        return result
    def backtracking(self, n, k, startIndex, path, result):
        if len(path) == k: 
            result.append(path[:]) 
            return
        for i in range(startIndex, n - (k - len(path)) + 2):  # 优化的地方
            path.append(i)  
            self.backtracking(n, k, i + 1, path, result) 
            path.pop()  

#优化处的思考逻辑：
#已经选取的元素个数：len(path)，比如n=5,k=3；这一行里面分别是12345；2345；345；45；5.只有12345；2345；345这3种情况满足要求。（45后面没得选了，k就不会到3了，叶子节点是不满足条件的，直接剪枝，同理5后面也没得选了）--我的最小起始点可以是1（startIndex的初始值），我的最大起始点可以是3.所以我的loop应该是range(1,4)
#3是怎么得到的呢？：元素总个数5-可以选择的元素个数3+1
#所以我的range的右边界应该是：元素总个数n - 还要选的元素(k-len(path)) + 1 + 1
#range的右边界 = n - (k - len(path)) + 2
```
