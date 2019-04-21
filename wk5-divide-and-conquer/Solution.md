# Week 5: Divide and Conquer

## Problem 1
[LEETCODE 23: Merge K Sorted Arrays](https://leetcode.com/problems/merge-k-sorted-lists/)
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):            
    def mergeKLists(self, lists):
        if not lists:
            return
        if len(lists) == 1:
            return lists[0]
        mid = len(lists)//2
        l = self.mergeKLists(lists[:mid])
        r = self.mergeKLists(lists[mid:])
        return self.merge(l, r)

    def merge(self, l, r):
        mergelist = cur = ListNode(0)
        while l and r:
            if l.val < r.val:
                cur.next = l
                l = l.next
            else:
                cur.next = r
                r = r.next
            cur = cur.next
        cur.next = l or r
        return mergelist.next   
```

## Problem 2
[LEETCODE 43: Multiply Strings](https://leetcode.com/problems/multiply-strings/)

## Problem 3
[LEETCODE 4: Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)
```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        n1 = len(nums1)
        n2 = len(nums2)
        n = n1 + n2


        def helper(k, s1, s2, odd):
            if s1 >= n1:
                if odd == True:
                    return nums2[s2 + k]
                else:
                    return (nums2[s2 + k - 1] + nums2[s2 + k])/2.0
            if s2 >= n2:
                if odd == True:
                    return nums1[s1 + k]
                else:
                    return (nums1[s1 + k - 1] + nums1[s1 + k])/2.0
            if k == 1:
                if nums1[s1] < nums2[s2]:
                    a = nums1[s1]
                    b = min(nums1[s1 + 1], nums2[s2]) if s1 + 1 < n1 else nums2[s2]
                else:
                    a = nums2[s2]
                    b = min(nums1[s1], nums2[s2 + 1]) if s2 + 1 < n2 else nums1[s1]
                if odd == True:
                    return b
                else:
                    return (a + b)/2.0

            mid = k//2 - 1
            t1 = min(s1 + mid, n1 - 1)
            t2 = min(s2 + mid, n2 - 1)
            if nums1[t1] < nums2[t2]:
                k = k - t1 + s1 - 1
                s1 = t1 + 1              
            else:
                k = k - t2 + s2 - 1
                s2 = t2 + 1
            return helper(k, s1, s2, odd)


        return helper(n//2, 0, 0, n%2==1)

```

## Problem 4
[LEETCODE 215: Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
```python
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        low = min(nums)
        high = max(nums)

        if high == 1:
            return nums[0]

        def count(a):
            m = 0
            for num in nums:
                if num > a: m += 1
            return m

        while low < high:
            mid = (low + high)//2
            if count(mid) >= k:
                low = mid + 1
            else:
                high = mid

        return low
```

## Problem 5
[LEETCODE 240: Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
```python
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if len(matrix) >= 1:
            m = len(matrix)
            n = len(matrix[0])
        else:
            return False

        def helper(i, j):
            if i >= m or j < 0:
                return False
            if matrix[i][j] == target:
                return True

            if matrix[i][j] < target:
                return helper(i + 1, j)
            else:
                return helper(i, j - 1)

        return helper(0, n - 1)
```

## Problem 6
[LEETCODE 932: Beautiful Array](https://leetcode.com/problems/beautiful-array/)
