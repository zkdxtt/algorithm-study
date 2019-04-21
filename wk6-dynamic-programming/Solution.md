# Week 6: Dynamic Programming

## Problem 1
[Leetcode 85: Maximal Rectangles](https://leetcode.com/problems/maximal-rectangle/)
```python
class Solution(object):
    def maximalRectangle(self, matrix):
        """
        :type matrix: List[List[str]]
        :rtype: int
        """
        if not matrix or not matrix[0]: return 0
        M, N = len(matrix), len(matrix[0])
        height = [0] * N
        res = 0
        for row in matrix:
            for i in range(N):
                if row[i] == '0':
                    height[i] = 0
                else:
                    height[i] += 1
            res = max(res, self.maxRectangleArea(height))
        return res            

    def maxRectangleArea(self, height):
        if not height: return 0
        res = 0
        stack = list()
        height.append(0)
        for i in range(len(height)):
            cur = height[i]
            while stack and cur < height[stack[-1]]:
                w = height[stack.pop()]
                h = i if not stack else i - stack[-1] - 1
                res = max(res, w * h)
            stack.append(i)
        return res
```
## Problem 2
[Leetcode 188: Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)
```python
class Solution(object):
    def maxProfit(self, k, prices):
        """
        :type k: int
        :type prices: List[int]
        :rtype: int
        """
        if k <= 0 or not prices: return 0
        N = len(prices)
        if k >= N:
            _sum = 0
            for i in xrange(1, N):
                if prices[i] > prices[i - 1]:
                    _sum += prices[i] - prices[i - 1]
            return _sum
        g = [0] * (k + 1)
        l = [0] * (k + 1)
        for i in xrange(N - 1):
            diff = prices[i + 1] - prices[i]
            for j in xrange(k, 0, -1):
                l[j] = max(g[j - 1] + max(diff, 0), l[j] + diff)
                g[j] = max(l[j], g[j])
        return g[-1]
```

## Problem 3
[Leetcode 139: Word Break](https://leetcode.com/problems/word-break/)
```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        print(s)
        print(wordDict)
        dp = [False] * (len(s) + 1)
        dp[0] = True
        print(dp)
        for i in xrange(1, len(s) + 1):
            for k in xrange(i):
                if dp[k] and s[k:i] in wordDict:
                    dp[i] = True
                    print(dp)
        return dp.pop()
```
## Problem 4
[Leetcode 152: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
```python
class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums: return 0
        N = len(nums)
        f = [0] * N
        g = [0] * N
        f[0] = g[0] = res = nums[0]
        for i in range(1, N):
            f[i] = max(f[i - 1] * nums[i], nums[i], g[i - 1] * nums[i])
            g[i] = min(f[i - 1] * nums[i], nums[i], g[i - 1] * nums[i])
            res = max(res, f[i])
        return res
```

## Problem 5
[Leetcode 213. House Robber II](https://leetcode.com/problems/house-robber-ii/)
```python
class Solution:
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums: return 0
        if len(nums) == 1: return nums[0]
        if len(nums) == 2: return max(nums[0], nums[1])
        N = len(nums)
        return max(self.rob_range(nums[0 : N - 1]), self.rob_range(nums[1 : N]))

    def rob_range(self, nums):
        if not nums: return 0
        if len(nums) == 1: return nums[0]
        if len(nums) == 2: return max(nums[0], nums[1])
        N = len(nums)
        dp = [0] * N
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])
        for i in range(2, N):
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i])
        return dp[-1]
```


## Problem 6
[Leetcode 639. Decode Ways II](https://leetcode.com/problems/decode-ways-ii/)
