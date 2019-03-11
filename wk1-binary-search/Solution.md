# Week 1: Binary Search

## Problem 1
[Leetcode 81: Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: bool
        """
        low = 0
        high = len(nums) - 1
        while low <= high:

            while nums[high] == nums[low] and high > low:
                high -= 1

            mid = low + (high - low)/2

            if nums[mid] == target:
                return True

            if nums[low] <= nums[mid]:
                if nums[low] <= target and target < nums[mid]:
                    high = mid - 1
                else:
                    low = mid + 1
            else:
                if nums[mid] < target and target <= nums[high]:
                    low = mid + 1
                else:
                    high = mid - 1
        return False


```

## Problem 2
[Leetcode 278: First Bad Version](https://leetcode.com/problems/first-bad-version/)

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        low, high = 1, n
        while low <= high:
            mid = low + (high - low)//2
            if isBadVersion(mid) == False:
                low = mid + 1
            else:
                high = mid - 1
        return low


```

## Problem 3
[Leetcode 744: Find Smallest Letter Greater Than Target](https://leetcode.com/problems/find-smallest-letter-greater-than-target/)

```python
class Solution(object):
    def nextGreatestLetter(self, letters, target):
        """
        :type letters: List[str]
        :type target: str
        :rtype: str
        """
        low = 0
        high = len(letters) - 1
        while low <= high:
            mid = low + (high - low)//2
            if letters[mid] > target:
                high = mid - 1
            else:
                low = mid + 1
        return letters[(high + 1) % len(letters)]

```

## Problem 4
[Leetcode 153: Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

```python
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        low = 0
        high = len(nums) - 1

        while low < high:
            if nums[low] < nums[high]:
                return nums[low]
            mid = low + (high - low)//2
            if nums[mid] > nums[high]:
                low = mid + 1
            else:
                high = mid
        return nums[low]

```

## Problem 5
[Leetcode 378: Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

```python
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        n = len(matrix[0])
        l = matrix[0][0]
        r = matrix[-1][-1]


        def findSmaller(nums, target):
            low = 0
            high = len(nums) - 1
            while low <= high :
                mid = low + (high - low)//2
                if nums[mid] <= target:
                    low = mid + 1
                else:
                    high = mid - 1
            return low


        while l <= r:
            m = l + (r - l)//2
            total_smaller = 0
            for i in range(n):
                total_smaller += findSmaller(matrix[i], m)
            if total_smaller >= k:
                r = m - 1
            else:
                l = m + 1


        return l

```

## Problem 6
[Leetcode 410: Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

```python
class Solution(object):
    def splitArray(self, nums, m):
        """
        :type nums: List[int]
        :type m: int
        :rtype: int
        """
        low = max(nums)
        high = sum(nums)
        n = len(nums)
        while low < high :
            mid = low + (high - low)//2
            subsum = 0
            count=0
            for i in range(n):
                subsum += nums[i]
                if subsum > mid:
                    subsum = nums[i]
                    count += 1
                elif subsum == mid:
                    subsum = 0
                    count += 1
            if subsum != 0:
                count += 1
            if count > m:
                low = mid + 1
            else:
                high = mid
        return low
```
