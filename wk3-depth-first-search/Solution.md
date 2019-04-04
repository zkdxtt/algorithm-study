# Week 3: Depth First Search

## Problem 1
101. Symmetric Tree
https://leetcode.com/problems/symmetric-tree/
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        def sym(node1, node2):
            if node1 is None:
                if node2 is None:
                    return True
                else:
                    return False
            if node2 is None:
                return False

            if node1.val != node2.val:
                return False

            return sym(node1.left, node2.right) and sym(node1.right, node2.left)

        if root is None:
            return True
        return sym(root, root)

````

## Problem 2
105. Construct Binary Tree from Preorder and Inorder Traversal
https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if not preorder or not inorder:
            return None

        rootValue = preorder.pop(0)
        root = TreeNode(rootValue)
        inorderIndex = inorder.index(rootValue)

        root.left = self.buildTree(preorder, inorder[:inorderIndex])
        root.right = self.buildTree(preorder, inorder[inorderIndex+1:])

        return root

```

## Problem 3
113. Path Sum II
https://leetcode.com/problems/path-sum-ii/
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def pathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: List[List[int]]
        """
        result = []
        def dfs(root, cusum, path):
            if root is None:
                return True
            cusum = cusum + root.val
            path = path + [root.val]
            left = dfs(root.left, cusum, path)
            right = dfs(root.right, cusum, path)
            if left and right:
                if cusum == sum:
                    result.append(path)
            return False
        dfs(root,0,[])
        return result
```

## Problem 4
207. Course Schedule
https://leetcode.com/problems/course-schedule/
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

## Problem 5
988. Smallest String Starting From Leaf
https://leetcode.com/problems/smallest-string-starting-from-leaf/
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def smallestFromLeaf(self, root):
        """
        :type root: TreeNode
        :rtype: str
        """

        letters = 'abcdefghijklmnopqrstuvwxyz'
        res = []
        def dfs(root, path, res):
            if root is None:
                return 1
            path = letters[root.val] + path
            left = dfs(root.left, path, res)
            right = dfs(root.right, path, res)
            if left and right:
                res += [path]
            return 0
        dfs(root, '', res)
        res.sort()
        return res[0]

```

## Problem 6
980. Unique Paths III
https://leetcode.com/problems/unique-paths-iii/
```python
class Solution(object):
    def uniquePathsIII(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        nrow = len(grid)
        ncol = len(grid[0])
        direction = [(-1, 0), (0, -1), (0, 1), (1, 0)]
        ans = []
        #establish a state matrix
        state_mtx = []
        for i in range(nrow):
            state_mtx += [[0] * ncol]

        #find start position
        for i in range(nrow):
            for j in range(ncol):
                if grid[i][j] == 1:
                    start_pos = [i, j]
                if grid[i][j] == -1:
                    state_mtx[i][j] = 1


        def AllOne(state_mtx):
            for i in range(nrow):
                for j in range(ncol):
                    if state_mtx[i][j] != 1:
                        return False
            return True

        def dfs(start_pos, state_mtx, ans):

            start_pos_i = start_pos[0]
            start_pos_j = start_pos[1]
            if state_mtx[start_pos_i][start_pos_j] == 1:
                return False
            state_mtx[start_pos_i][start_pos_j] = 1

            if grid[start_pos_i][start_pos_j] == 2:
                if AllOne(state_mtx):
                    ans += [0]                
                state_mtx[start_pos_i][start_pos_j] = 0
                return True             

            for i, j in direction:
                if 0 <= start_pos_i + i < nrow and 0 <= start_pos_j + j < ncol:
                    dfs([start_pos_i + i, start_pos_j + j], state_mtx, ans)
            state_mtx[start_pos_i][start_pos_j] = 0

        dfs(start_pos, state_mtx, ans)
        return len(ans)
```
