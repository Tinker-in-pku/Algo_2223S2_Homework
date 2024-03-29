## AIgo_Homework_10

唐浩然 

2201111746



#### LeetCode 787

![Leetcode_787](./pic/Leetcode_787.png)

**code：**

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        cost = [1000000 for i in range(n)]
        cost[src] = 0
        dist = [src]
        while(k >= 0):
            k = k - 1
            dist_i = []
            cost_i = cost.copy()
            for fi in flights:
                if fi[0] in dist:
                    dist_i.append(fi[1])
                    cost_i[fi[1]] = min(min(cost[fi[1]],cost[fi[0]] + fi[2]),cost_i[fi[1]]) 
            dist = list(set(dist_i))
            cost = cost_i.copy()
        return -1 if cost[dst] == 1000000 else cost[dst]
```

**算法思路：**

直接对于给定的图进行BFS搜索即可，每进行一次搜索即步数加一；

#### Leetcode 934

![Leetcode_934](./pic/Leetcode_934.png)

**code:**

```python
class Solution:
    def islandDetect(self, i, j):
        self.island.append((i,j))
        self.visited[i][j] = 1
        self.grid[i][j] = 0
        for k in range(4):
            if 0 <= i+self.xi[k] < self.n and 0 <= j+self.yi[k] < self.n and self.grid[i+self.xi[k]][j+self.yi[k]] == 1:
                self.islandDetect(i+self.xi[k],j+self.yi[k])

    def shortestBridge(self, grid: List[List[int]]) -> int:
        self.grid = grid
        self.n = len(grid)
        self.xi = [1,-1,0,0]
        self.yi = [0,0,-1,1]
        self.island = []
        self.visited = [[0 for i in range(self.n)] for j in range(self.n)]
        for i in range(self.n):
            for j in range(self.n):
                if self.grid[i][j] == 1:
                    self.islandDetect(i,j)
                    break
            if len(self.island) != 0 :
                break
        ans = 0
        is_connected = False
        while not is_connected:
            ans += 1
            BFS_queue = []
            for (i,j) in self.island:
                for k in range(4):
                    if 0 <= i+self.xi[k] < self.n and 0 <= j+self.yi[k] < self.n and self.visited[i+self.xi[k]][j+self.yi[k]] == 0:
                        self.visited[i+self.xi[k]][j+self.yi[k]] = 1
                        BFS_queue.append((i+self.xi[k],j+self.yi[k]))
                        if self.grid[i+self.xi[k]][j+self.yi[k]] == 1:
                            is_connected = True
                            return ans - 1
            self.island = BFS_queue.copy()

        return ans

```

**算法思路：**

同样是BFS搜索，第一步先用BFS确定其中一个岛屿，之后对于岛屿中的所有点进行BFS搜索，对已经搜索过的点进行标记，之后第一次搜索到 “1” 时的搜索步数即为最短连通路径；

#### LeetCode 692

![Leetcode](./pic/Leetcode_692.png)

**code:**

```python
class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        hash_dict = {}
        for word in words:
            if word not in hash_dict.keys():
                hash_dict[word] = 1
            else:
                hash_dict[word] += 1
        freq = [[] for i in range(len(words))]
        re = []
        cnt = 0
        for ki,vi in hash_dict.items():
            freq[vi].append(ki)
        for li in freq[::-1]:           
            if cnt >= k:
                break
            li.sort(reverse=False)
            for w in li:
                re.append(w)
                cnt += 1
                if cnt >= k:
                    break
        return re
```

**算法思路：**

首先利用字典对于单词词频进行统计，之后利用频率次数作为数组索引进行基数排序，从大到小取前k个即可；
