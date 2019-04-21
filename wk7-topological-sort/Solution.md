# Week 7: Topological Sort

## Problem 1
[207: Course Schedule](https://leetcode.com/problems/course-schedule/)
```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        graph =  collections.defaultdict(list)
        for u, v in prerequisites:
            graph[u].append(v)

        visited = [0] * numCourses

        def dfs(visited, i):
            if visited[i] == 1:
                return False
            if visited[i] == 2:
                return True
            visited[i] = 1
            
            for j in graph[i]:
                if not dfs(visited, j):
                    return False
            visited[i] = 2
            return True

        for i in range(numCourses):
            if not dfs(visited, i):
                return False
        return True
```

## Problem 2
[210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
```python
class Solution(object):
    def findOrder(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: List[int]
        """
        if numCourses == 0:
            return []

        v_in = [0 for i in range(numCourses)]
        v_out = [[] for i in range(numCourses)]

        for edge in prerequisites:
            nd1, nd2 = edge[0], edge[1]
            v_out[nd2].append(nd1)
            v_in[nd1] += 1

        queue = [i for i,x in enumerate(v_in) if x==0]
        result = []
        while queue:
            cur_nd = queue.pop(0)
            result.append(cur_nd)
            for child in v_out[cur_nd]:
                v_in[child] -=1
                if v_in[child] == 0:
                    queue.append(child)

        return result if len(result) == numCourses else []
```

## Problem 3
[802: Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)
```python
class Solution(object):
    def eventualSafeNodes(self, graph):
        """
        :type graph: List[List[int]]
        :rtype: List[int]
        """
        num_nodes = len(graph)
        if num_nodes == 0:
            return []

        v_out = [0 for i in range(num_nodes)]
        v_ancestor = [[] for i in range(num_nodes)]

        for i in range(num_nodes):
            v_out[i] = len(graph[i])
            children = graph[i]
            for child in children:
                v_ancestor[child].append(i)

        queue = [i for i,x in enumerate(v_out) if x == 0]
        result = []

        while queue:
            curnode = queue.pop(0)
            result.append(curnode)
            for parent in v_ancestor[curnode]:
                v_out[parent] -= 1
                if v_out[parent] == 0:
                    queue.append(parent)

        result.sort()
        return result
```

## Problem 4
[329: Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)
```python
class Solution(object):
    def longestIncreasingPath(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: int
        """
        def dfs(i, j):
            if not dp[i][j]:
                val = matrix[i][j]
                dp[i][j] = 1 + max(0,
                    dfs(i - 1, j) if i and val > matrix[i - 1][j] else 0,
                    dfs(i + 1, j) if i < M - 1 and val > matrix[i + 1][j] else 0,
                    dfs(i, j - 1) if j and val > matrix[i][j - 1] else 0,
                    dfs(i, j + 1) if j < N - 1 and val > matrix[i][j + 1] else 0)
            return dp[i][j]

        if not matrix or not matrix[0]: return 0
        M, N = len(matrix), len(matrix[0])
        dp = [[0]*N for i in range(M)]
        return max(dfs(x,y) for x in range(M) for y in range(N))
```
