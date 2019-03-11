# Week 2: Breadth First Search

## Problem 1
Binary Tree Right Side View (199)  
https://leetcode.com/problems/binary-tree-right-side-view/
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from collections import deque
class Solution(object):
    def rightSideView(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if root is None:
            return []
        queue = deque([root])
        result = []
        while queue:
            layer = []
            for eachnode in range(len(queue)):
                curnode = queue.popleft()
                layer.append(curnode.val)
                if curnode.left != None:
                    queue.append(curnode.left)
                if curnode.right != None:
                    queue.append(curnode.right)
            result.append(layer.pop())
        return result

```

## Problem 2
Maximum Depth of Binary Tree (559)  
https://leetcode.com/problems/maximum-depth-of-n-ary-tree/
```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val, children):
        self.val = val
        self.children = children
"""
from collections import deque
class Solution(object):
    def maxDepth(self, root):
        """
        :type root: Node
        :rtype: int
        """
        if root is None:
            return 0
        queue = deque([root])
        result = 0
        while queue:
            result += 1
            for eachnode in range(len(queue)):
                curnode = queue.popleft()
                if curnode.children != None:
                    for node in curnode.children:
                        queue.append(node)
        return result
```

## Problem 3
Find Bottom Left Tree Value (513)  
https://leetcode.com/problems/find-bottom-left-tree-value/
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import deque
class Solution(object):
    def findBottomLeftValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return []
        queue = deque([root])
        while queue:
            layer = []
            for eachnode in range(len(queue)):
                curnode = queue.popleft()
                layer.append(curnode.val)
                if curnode.left != None:
                    queue.append(curnode.left)
                if curnode.right != None:
                    queue.append(curnode.right)
        return layer[0]
```

## Problem 4
Binary Tree Zigzag Level Order Traversal (103)  
https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import deque
class Solution(object):
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if root is None:
            return []
        queue = deque([root])
        result = []
        flag = 0
        while queue:
            layer = []
            for eachnode in range(len(queue)):
                curnode = queue.popleft()
                layer.append(curnode.val)
                if curnode.left != None:
                    queue.append(curnode.left)
                if curnode.right != None:
                    queue.append(curnode.right)
            if flag:
                reverse = []
                while layer:
                    reverse.append(layer.pop())
                result.append(reverse)
            else:
                result.append(layer)
            flag = (flag + 1)%2
        return result
```

## Problem 5
Employee Importance (690)  
https://leetcode.com/problems/employee-importance/
```python
"""
# Employee info
class Employee(object):
    def __init__(self, id, importance, subordinates):
        # It's the unique id of each node.
        # unique id of this employee
        self.id = id
        # the importance value of this employee
        self.importance = importance
        # the id of direct subordinates
        self.subordinates = subordinates
"""
from collections import deque
class Solution(object):
    def getImportance(self, employees, id):
        """
        :type employees: Employee
        :type id: int
        :rtype: int
        """

        def helper(id):
            res = dic[id]
            for sub_id in sub[id]:
                res += helper(sub_id)
            return res

        dic = {}
        sub = {}
        for employee in employees:
            dic[employee.id] = employee.importance
            sub[employee.id] = employee.subordinates
        return helper(id)      
```

## Problem 6
Minesweeper (529)   
https://leetcode.com/problems/minesweeper/
```python
from collections import deque
class Solution(object):
    def updateBoard(self, board, click):
        """
        :type board: List[List[str]]
        :type click: List[int]
        :rtype: List[List[str]]
        """
        # boundary
        i, j = click
        if board[i][j] == "M":
            board[i][j] = "X"
            return board
        # initialization
        numbers = "B123456789"
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1), (1, 1), (-1, -1), (1, -1), (-1, 1)]
        queue = deque([click])
        while queue:
            i, j = queue.popleft()
            if board[i][j] == "B":
                continue
            mineCnt = 0
            nbrs = []
            for di, dj in directions:
                ni, nj = i + di, j + dj
                if 0 <= ni < len(board) and 0 <= nj < len(board[0]):
                    if board[ni][nj] == "M":
                        mineCnt += 1
                    elif board[ni][nj] == "E":
                        nbrs.append((ni, nj))
            if mineCnt == 0:
                queue.extend(nbrs)
            board[i][j] = numbers[mineCnt]
        return board
```
