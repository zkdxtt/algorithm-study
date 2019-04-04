# Week 4: Back Tracking

## Problem 1
[Leetcode 90: Subsets II](https://leetcode.com/problems/subsets-ii/)
```python
class Solution(object):
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        result = []
        nums.sort()
        N = len(nums)
        def dfs(index, temp):
            if temp not in result:
                result.append(temp)
            for i in range(index, N):
                dfs(i + 1, temp + [nums[i]])
        dfs(0, [])
        return result
```

## Problem 2
[Leetcode 52: N-Queens II](https://leetcode.com/problems/n-queens-ii/)
```python

```

## Problem 3
[Leetcode 216: Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)
```python
class Solution(object):
    def combinationSum3(self, k, n):
        """
        :type k: int
        :type n: int
        :rtype: List[List[int]]
        """
        result = []

        def dfs(num, m, cusum, temp):
            if cusum > n:
                return False
            if m == k:
                if cusum == n:
                    result.append(temp)
                    return False
                return True
            for i in range(num, 10):
                if dfs(i + 1, m + 1, cusum + i, temp + [i]) is False:
                    return True
            return True

        dfs(1, 0, 0, [])

        return result
```

## Problem 4
[Leetcode 37: Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)
```python
```


## Problem 5
[Leetcode 47: Permutations II](https://leetcode.com/problems/permutations-ii/)
```python
class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """


        def permu(numbers):
            a = numbers.pop()
            k = len(numbers)
            add_a = []
            if k == 0:
                return [[a]]
            not_a = permu(numbers)

            for j in range(len(not_a)):
                term = not_a[j]
                for i in range(k+1):
                    temp = term[0:i] + [a] + term[i:k]
                    if temp not in add_a:
                        add_a.append(temp)
            return add_a

        return permu(nums)
```

## Problem 6
[Leetcode 131: Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)
```python
class Solution(object):
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """
        def isPalindrome(s):
            i = 0
            for i in range(len(s)/2):
                if s[i] != s[len(s) - i - 1]:
                    return False
            return True

        result = []

        def dfs(partition, m):
            k = len(partition)
            is_partition = True
            for i in range(m, len(partition)):
                term = partition[i]
                length = len(term)
                if length > 1 :
                    for j in range(1, length):
                        if isPalindrome(partition[i][0:j]):
                            dfs(partition[0:i] + [partition[i][0:j]] + [partition[i][j:length]] + partition[(i+1):k], i + 1)
                if isPalindrome(term) is False:
                    is_partition = False
                    break
            if is_partition:
                result.append(partition)

        dfs([s], 0)
        return result
```
