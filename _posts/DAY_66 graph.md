# DFS

DFS的代码框架：

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
        dirs = [(-1, 0), (0, 1), (1, 0), (0, -1)]  # 四个方向
        result = 0

        def dfs(x, y):
            if visited[x][y] or grid[x][y] == '0':
                return  # 终止条件：访问过的节点 或者 遇到海水
            visited[x][y] = True
            for d in dirs:
                nextx = x + d[0]
                nexty = y + d[1]
                if nextx < 0 or nextx >= m or nexty < 0 or nexty >= n:  # 越界了，直接跳过
                    continue
                dfs(nextx, nexty)
        
        for i in range(m):
            for j in range(n):
                if not visited[i][j] and grid[i][j] == '1':
                    result += 1  # 遇到没访问过的陆地，+1
                    dfs(i, j)  # 将与其链接的陆地都标记上 true

        return result
```

# BFS

### BFS模板：

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
