---
title: Leetcode Weekly Contest 204
date: 2020-08-30 00:49:56
tags:
- Competitive Programming
- Algorithm
categories:
- Competitive Programming
- Algorithm
---

### Overview

**Rank**: 599/13949

**Score**: 13(Q1~3)

**Finish Time**: 0:51:39

This contest is interesting.  Q3 is tricky and Q4 is worth trying.

### Q2 Maximum Length of Subarray With Positive Product

https://leetcode.com/problems/maximum-length-of-subarray-with-positive-product/

This is a typical dp problem. When it's about max/min length of subarray, you can try to use dynamic programming and define dp array like the max/min length ending with index i.

My solution is below. O(n) for time complexity and O(n) for space complexity.

But we can easily improve it into O(1) space complexity.

```
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [[0, 0] for _ in range(n)]
        
        if nums[0] > 0: dp[0][0] = 1
        elif nums[0] < 0: dp[0][1] = 1
        
        for i in range(1, n):
            if nums[i] == 0:
                continue
            elif nums[i] > 0:
                dp[i][0] = dp[i - 1][0] + 1
                dp[i][1] = dp[i - 1][1] + 1 if dp[i - 1][1] > 0 else 0
            else:
                dp[i][0] = dp[i - 1][1] + 1 if dp[i - 1][1] > 0 else 0
                dp[i][1] = dp[i - 1][0] + 1
        
        return max([x[0] for x in dp])
```

### Q3 Minimum Number of Days to Disconnect Island

https://leetcode.com/problems/minimum-number-of-days-to-disconnect-island/

This one is really tricky.

At first it feel like we need to find the bridge and use algorithm like Tarjan. But then I find that the result of days will not be greater than 2! Just think about corner of connected component.

Thus we just need to enumerate 0, 1, 2 this three condition.

0 means there are multiple island. 1 means we can delete one '1' and it's not fully connected. Use bfs to check that. If it's not 0 nor 1, then it must be 2!

Below is my solution. O(m^2*n^2) for time and space complexity.

```
from collections import deque
class Solution:
    def minDays(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        cnt_ones = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    cnt_ones += 1
        
        if cnt_ones == 0: return 0
        elif cnt_ones == 1: return 1
        
        bfs_cnt = self.bfs(grid)
        if bfs_cnt != cnt_ones: return 0
        
        # whether only use 1 day
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    grid[i][j] = 0
                    bfs_cnt = self.bfs(grid)
                    if bfs_cnt != cnt_ones - 1: return 1
                    grid[i][j] = 1
        return 2
                    
    def bfs(self, grid):# return cnt of ones
        m, n = len(grid), len(grid[0])
        start = None
        move = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    start = (i, j)
                    break
            if grid[i][j]: break
        
        queue = deque([start])
        visited = set()
        visited.add(start)
        
        while queue:
            x, y = queue.popleft()
            for dx, dy in move:
                nx, ny = x + dx, y + dy
                if not (0 <= nx < m and 0 <= ny < n and grid[nx][ny]): continue
                np = (nx, ny)
                if np in visited: continue
                queue.append(np)
                visited.add(np)
        
        return len(visited)
```



### Q4 Number of Ways to Reorder Array to Get Same BST

https://leetcode.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/

This one is interesting and not that hard. Actually I got the right approach and feelings but messed up some parts.

"When it comes to BST, this about root and its left and right child's relationship."

This problem is also following that thinking.

It's about how many reorder of left and right and how many ways to combine left and right's order nums!

Below is the implementation. Not sure about time and space complexity.

```
from math import comb
class Solution:
    def numOfWays(self, nums: List[int]) -> int:
        mod = 10 ** 9 + 7
        def cnt(nums):
            n = len(nums)
            if n <= 1: return n
            left = [x for x in nums if x < nums[0] ]
            right = [x for x in nums if x > nums[0]]
            
            left_cnt, right_cnt = cnt(left), cnt(right)
            # in case there are 0 for cnt
            if not left_cnt: return right_cnt
            if not right_cnt: return left_cnt
            
            return (comb(len(left) + len(right), len(right)) * left_cnt * right_cnt) % mod
            
        return cnt(nums) - 1
```







