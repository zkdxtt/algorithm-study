# Week 8: Bit Manipulation

## Problem 1
[268. Missing Number][https://leetcode.com/problems/missing-number/]
```python
class Solution(object):
    def missingNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        res= len(nums)
        for i,num in enumerate(nums):
            res = res^i^num
        return res
```


## Problem 2
[187. Repeated DNA Sequences][https://leetcode.com/problems/repeated-dna-sequences/]
```python
class Solution(object):
    def findRepeatedDnaSequences(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        graph = {'A':0,'C':1,'G':2,'T':3}
        n=len(s)
        code = 0

        if n<=10:
            return []

        for i in range(0,10):
            code = (code<<2)+graph[s[i]]
        dic =set()
        dic.add(code)
        res_code=[]
        res=[]
        for i in range(10,n):
            code = code&~(1<<19)
            code = code&~(1<<18)
            code = (code<<2) +graph[s[i]]
            if code in dic and code not in res_code:
                res.append(s[i-9:i+1])
                res_code.append(code)
            else:
                dic.add(code)
        return res
```




## Problem 3
[260. Single Number III][https://leetcode.com/problems/single-number-iii/]
```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        all_num = 0
        for num in nums:
            all_num^=num

        a=all_num
        b=all_num

        mask = all_num&(-all_num)

        for num in nums:
            if (mask&num):
                a^=num
            else:
                b^=num
        return [a,b]

```
