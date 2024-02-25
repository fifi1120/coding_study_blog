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
            return count # 和200比多加的一行

        for i in range(m): 
            for j in range(n):
                if grid[i][j] == 1 and not visited[i][j]:
                    areas.append(dfs(i, j, 0))
        
        if areas == []:
            return 0
        else:
            return max(areas)
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
