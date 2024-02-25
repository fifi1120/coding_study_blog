# DFS

## DFS的代码框架：

```
def dfs(graph, root, path, res):
    if 终止条件:
        *possible 处理（把path装进res）
        return

    for 选择 in 本节点所连接的其他节点:
        *possible 处理（把node装进path）
        dfs(graph, 选择的节点, path, res) # 递归
        *possible 回溯，撤销处理结果（如果是要存储路径等，就要pop；如果只是看是否visit，就不用）
```

三部曲：
1. 确认参数
2. 确认终止条件
3. for loop：本节点连接的所有其他节点

## DFS模板题： 

### 797 可能路径

```
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        res = []
        path = [0]
        if not graph:
            return []
        self.dfs(graph, 0, path, res)
        return res
    
    def dfs(self, graph, root:int, path, res):
        if root == len(graph) - 1:
            res.append(path[:])
            return
        
        for node in graph[root]:
            path.append(node)
            self.dfs(graph, node, path, res)
            path.pop()
```

### 200 岛屿数量 
```
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        visited = [[False] * n for _ in range(m)]
        dirs = [(-1,0), (0,1), (1,0), (0,-1)] # 里面是列表也可以：dirs = [[0,1], [1,0], [0,-1], [-1,0]]
        res = 0
        # 这四行要直接背下来
        # 其实这道题的本质是：用nested for loop计划走遍每一个方块，遇到没有访问过的陆地就+1，
        # 然后通过DFS递归地把相邻的陆地全部visit（遇到海水或已访问的就停止）。然后下一次再碰到新的陆地，就是独立岛屿，再+1。
        
        def dfs(x, y): # 把dfs写在里面有一个好处，就是不用再把grid等带着写。。
            if x < 0 or x >= m or y <0 or y >= n or grid[x][y] == "0" or visited[x][y]:
                return
            visited[x][y] = True # 你这个visited一定要写在下面，只有陆地才能被标记“visited”，如果都标记就达不到visited区别是不是“连着”的作用
            for d in dirs:
                nextx = x + d[0]
                nexty = y + d[1]
                dfs(nextx, nexty)
                
        for i in range(m):
            for j in range(n):
                if grid[i][j] == "1" and (not visited[i][j]):
                    res += 1
                    dfs(i,j)

        return res
```

### 695 最大岛屿面积

```
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        visited = [[False] * n for _ in range(m)]
        areas = []
        dirs = [[0,1],[1,0],[0,-1],[-1,0]]

        def dfs(x,y,count):
            if x >= m or y >= n or x < 0 or y < 0 or visited[x][y] or grid[x][y] == 0:
                return count
            visited[x][y] = True
            count += 1 # 和200比多加的一行 其实一次dfs本身的含义是：从陆地1走向连接的陆地2，所以如果要求岛屿面积，每执行一次dfs就要count+=1，dfs return的时候就是（碰到海水的时候），
                        # 也就是需要return count的时候
            for d in dirs:
                nextX = x + d[0]
                nextY = y + d[1]
                count = dfs(nextX, nextY, count)
            return count # 和200比多加的一行。这是因为你要把count传递给下一次的递归。
                         # 为什么回溯函数模板就不需要这样额外的return XXX。因为回溯函数通常会有额外的“存放结果”的步骤，我每次去存放结果，就可以算是在“传递”了。但这里是要做一个循环加法。

        for i in range(m): 
            for j in range(n):
                if grid[i][j] == 1 and not visited[i][j]:
                    areas.append(dfs(i, j, 0))
        
        if areas == []:
            return 0
        else:
            return max(areas)

在回溯算法中，通常会有一个“存放结果”的步骤，这个步骤是在递归过程中将符合条件的结果收集起来。由于这些结果通常是通过修改外部数据结构（如列表）来存储的，所以不需要通过函数返回值来“传递”结果。

回溯算法的典型用例是找到所有满足特定条件的解决方案，如所有可能的组合或排列。在这些情况下，当达到终止条件时，通常会有类似于 result.append(solution) 的操作，这里 solution 是当前的解决方案，而 result 是外部定义的用于存储所有解的列表。在回溯（撤销当前选择）之前，您将当前解添加到结果列表中。

在您的 DFS 题目（如岛屿的最大面积）中，情况略有不同。您的目标是计算每个岛屿的面积，并找到最大值。在这种情况下，您需要在递归过程中累积每个岛屿的面积，并将这个累积值传递回上层递归调用。这就是为什么您需要在 dfs 函数中返回 count 值的原因。因为没有类似 result.append(solution) 这样的操作来存储中间结果，所以必须依赖于返回值来实现结果的“传递”。

总结来说，是否需要在递归函数中使用返回值，取决于您如何处理和存储递归过程中生成的结果。在回溯算法中，由于结果通常是通过修改外部数据结构来存储的，所以不需要返回值。而在需要累积和传递中间结果的情况下，如您的 DFS 题目，返回值则是必需的。
```

### 417 太平洋

```
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        m, n = len(heights), len(heights[0])
        po = [[False] * n for _ in range(m)] # 这其实就是设定visited1，等下会直接放进dfs作为parameter
        ao = [[False] * n for _ in range(m)] # 这其实就是设定visited2，等下会直接放进dfs作为parameter
        dirs = [[0,1], [1,0], [0,-1], [-1,0]]
        res = []

        def dfs(visited, x, y):
            if x < 0 or y < 0 or x >= m or y >= n or visited[x][y]:
                return
            visited[x][y] = True
            for d in dirs:
                nextX = x + d[0]
                nextY = y + d[1]
                if nextX < 0 or nextY < 0 or nextX >= m or nextY >= n or visited[nextX][nextY]:
                    continue
                if heights[nextX][nextY] < heights[x][y]:
                    # 这一行不能移动到base case，因为要比较x和nextx的对应。而且由于你写了heights[next][nexty]，
                    # 那么在前面也需要重新判定nextx和nexty是否越界
                    continue
                dfs(visited, nextX, nextY)

# 要怎么<从低到高遍历>呢？PO的方向是从左、从上，AO的方向是从下、从右
        for i in range(m): # for loop行，能看整列
            dfs(po, i, 0) # 遍历PO的出发坐标：最左列【如何表示最左列：(i,0) * 行数】
            dfs(ao, i, n-1) # 遍历AO的出发坐标：最右列【如何表示最右列：(i, n-1) * 行数】

        for j in range(n):
            dfs(po, 0, j) # 遍历PO的出发坐标：第一行【如何表示第一行：(0,j) * 列数】
            dfs(ao, m-1, j) # 遍历AO的出发坐标：最后一行【如何表示最后一行：(m-1,j) * 列数】

        for i in range(m):
            for j in range(n):
                if po[i][j] and ao[i][j]:
                    res.append([i,j])
                else:
                    continue

        return res
```

# BFS

## BFS模板：

```
from collections import deque

dir = [(0, 1), (1, 0), (-1, 0), (0, -1)] # 创建方向元素

def bfs(grid, visited, x, y):
  
  queue = deque() # 初始化队列
  queue.append((x, y)) # 放入第一个元素/起点
  visited[x][y] = True # 标记为访问过的节点
  
  while queue: # 遍历队列里的元素
  
    curx, cury = queue.popleft() # 取出第一个元素
    
    for dx, dy in dir: # 遍历四个方向
    
      nextx, nexty = curx + dx, cury + dy
      
      if nextx < 0 or nextx >= len(grid) or nexty < 0 or nexty >= len(grid[0]): # 越界了，直接跳过
        continue
        
      if not visited[nextx][nexty]: # 如果节点没被访问过  
        queue.append((nextx, nexty)) # 加入队列
        visited[nextx][nexty] = True # 标记为访问过的节点
```

### 133 克隆图

```
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

from typing import Optional
class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        copyMap = {}

        def dfs(node):
            if node in copyMap:
                return copyMap[node]
            else:
                copy_node = Node(node.val)
                copyMap[node] = copy_node

            for nei in node.neighbors:
                copy_node.neighbors.append(dfs(nei)) 
                # copy_node.neighbors本身是一个空清单。
                # 把邻居加到copy_neighbors的清单里，本质上就是在“链接”。
                # 何时链接呢？就是已有复制品的时候（就return那个复制品），然后就可以copy_node.neighbors.append(复制品)就是连接了
            
            return copy_node # 最后为什么要有这样一句话呢？虽然在遇到base case的时候会return复制品，
            # 但是即使一个节点的副本最终会在 base case 中被返回，它的邻居节点可能不会立即达到这个条件。
            # 因此，对于每个邻居，dfs 函数都会返回它的副本，无论它是直接从 base case 返回的，还是在创建了新副本后返回的。


        if node == None:
            return None
        else:
            return dfs(node)
```

### BFS模板题

### 200 岛屿数量

```
class Solution:
    def __init__(self):
        self.dirs = [[0, 1], [1, 0], [-1, 0], [0, -1]] 
        
    def numIslands(self, grid: List[List[str]]) -> int:
        m = len(grid)
        n = len(grid[0])
        visited = [[False]*n for _ in range(m)]
        res = 0
        for i in range(m):
            for j in range(n):
                if visited[i][j] == False and grid[i][j] == '1':
                    res += 1
                    self.bfs(grid, i, j, visited)  # Call bfs within this condition
        return res

    def bfs(self, grid, i, j, visited):
        q = deque()
        q.append((i,j))
        visited[i][j] = True
        while q:
            x, y = q.popleft()
            for k in range(4):
                next_i = x + self.dirs[k][0]
                next_j = y + self.dirs[k][1]

                if next_i < 0 or next_i >= len(grid):
                    continue 
                if next_j < 0 or next_j >= len(grid[0]):
                    continue
                if visited[next_i][next_j]:
                    continue
                if grid[next_i][next_j] == '0':
                    continue
                q.append((next_i, next_j))
                visited[next_i][next_j] = True
```
