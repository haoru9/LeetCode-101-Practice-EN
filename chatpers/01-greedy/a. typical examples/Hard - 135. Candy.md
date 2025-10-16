# 135. Candy  
**Tags:** Greedy, Two-pass, Array  

---

## Problem Description  

There are `n` children standing in a line.  
Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have **at least one candy**.  
- Children with a **higher rating** get **more candies** than their neighbors.

Return the *minimum number of candies* you need to have to distribute the candies to the children.

---

## 中文题意  

有 `n` 个孩子站成一排，每个孩子都有一个评分。  
你需要分发糖果，要求如下：  
1. 每个孩子至少得到 **1 颗糖**；  
2. 评分更高的孩子比相邻的孩子得到 **更多的糖**。  

求分发糖果的最少总数量。

---

## Example 1  

**Input:**  
`ratings = [1,0,2]`  

**Output:**  
`5`  

**Explanation:**  
You can allocate candies as `[2,1,2]`.  
The middle child gets the least candy, while the others get more.  
Total = `2 + 1 + 2 = 5`.

---

## Example 2  

**Input:**  
`ratings = [1,2,2]`  

**Output:**  
`4`  

**Explanation:**  
You can allocate candies as `[1,2,1]`.  
The third child has the same rating as the second, so no extra candy is needed.  
Total = `1 + 2 + 1 = 4`.

---

## Constraints  

- `n == ratings.length`  
- `1 <= n <= 2 * 10^4`  
- `0 <= ratings[i] <= 2 * 10^4`

---

## Solution 1 — Two-pass Greedy (Recommended & Most Readable， O(n))

**Idea.**  
Satisfy left-neighbor constraints in a left→right pass, then satisfy right-neighbor constraints in a right→left pass.  
For each child `i`, the final number of candies is the **maximum** required by both directions.  
This ensures every rule is met with the smallest total.

**为什么可行（思路证明）：**  
- 左向扫描：保证每个上坡孩子（评分比左边高）得到更多糖果；  
- 右向扫描：保证每个下坡孩子（评分比右边高）得到更多糖果；  
- 对每个孩子取两方向要求的最大值，就能在同时满足两边的前提下保持最小总量。

---

### Python Code

```python
from typing import List

class Solution:
    def candy(self, ratings: List[int]) -> int:
        n = len(ratings)
        if n <= 1:
            return n

        # candies[i] records the minimum candies needed after satisfying
        # the LEFT constraint: if ratings[i] > ratings[i-1],
        # then candies[i] = candies[i-1] + 1
        candies = [1] * n

        # Pass 1: Left → Right
        for i in range(1, n):
            if ratings[i] > ratings[i - 1]:
                candies[i] = candies[i - 1] + 1

        # Pass 2: Right → Left
        # If ratings[i] > ratings[i+1], make sure candies[i] >= candies[i+1] + 1
        # Use max(...) so that we don't destroy the left-side rule.
        for i in range(n - 2, -1, -1):
            if ratings[i] > ratings[i + 1]:
                candies[i] = max(candies[i], candies[i + 1] + 1)

        # The sum of all candies is the minimal valid total.
        return sum(candies)
```

## Solution 2 — One-pass, O(1) Extra Space (Mountain Counting)

**Idea.**  
我们可以把评分序列看作若干个「山形」结构：先上升再下降。  
通过跟踪三个变量：  
- `up`：当前上升坡的长度；  
- `down`：当前下降坡的长度；  
- `peak`：上一个山峰的上升长度（峰顶高度）；  
我们可以在**一次遍历**中计算最少糖果数量，只使用 `O(1)` 额外空间。

每个孩子的处理逻辑如下：
- 当评分**上升**时：`up += 1`，加 `1 + up` 颗糖；  
- 当评分**持平**时：重置所有计数，加 `1`；  
- 当评分**下降**时：`down += 1`，加 `1 + down` 颗糖。  
  - 若下降段未超过峰顶长度，减去重复的那一颗；  
  - 若下降段更长，则相当于把峰顶再“抬高”一格。

这样，每个“山形”的最少糖果总数满足： sum(1..up)+sum(1..down)+max(up,down)+1

---

### Python Code

```python
from typing import List

class Solution:
    def candy(self, ratings: List[int]) -> int:
        n = len(ratings)
        if n <= 1:
            return n

        # Initial candy count
        candies = 1
        up = down = peak = 0

        for i in range(1, n):
            if ratings[i] > ratings[i - 1]:
                # rising slope
                up += 1
                peak = up
                down = 0
                candies += 1 + up
            elif ratings[i] == ratings[i - 1]:
                # flat slope
                up = down = peak = 0
                candies += 1
            else:
                # falling slope
                up = 0
                down += 1
                # avoid double counting the peak
                if peak >= down:
                    candies += down
                else:
                    candies += down + 1

        return candies
```

