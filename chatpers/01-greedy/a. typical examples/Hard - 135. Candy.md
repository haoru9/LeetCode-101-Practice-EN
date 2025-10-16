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


## Solution 2 — One-pass, O(1) Extra Space (Mountain Counting)

**Idea.**  
We can view the entire ratings sequence as a series of *mountains*:  
each mountain has an increasing slope followed by a decreasing slope.  
By tracking three variables —  
- `up`: length of the current increasing slope,  
- `down`: length of the current decreasing slope,  
- `peak`: the length of the last increasing slope (height of the mountain’s peak) —  
we can compute the total candies in a **single pass**, using only `O(1)` extra space.

For every child:
- When ratings **rise**, we increment `up` and add `1 + up` candies.  
- When ratings are **equal**, we reset all counters and add `1`.  
- When ratings **fall**, we increment `down` and add `1 + down` candies,  
  but if the current `down` length does **not exceed** `peak`,  
  we subtract `1` to avoid double-counting the peak’s candy.  
  If the downhill run is **longer than** the uphill, we keep the extra candy,  
  effectively “raising” the peak by one.

This ensures every “mountain” contributes the minimal number of candies:
\[
\text{sum}(1..up) + \text{sum}(1..down) + \max(up, down) + 1
\]

---

### Python Code

```python
from typing import List

class Solution:
    def candy(self, ratings: List[int]) -> int:
        n = len(ratings)
        if n <= 1:
            return n

        # 初始值：第一个孩子至少 1 颗糖
        min_number_of_candies = 1
        up = down = peak = 0

        for i in range(1, n):
            if ratings[i] > ratings[i - 1]:
                # 上升趋势：右边孩子比左边评分高
                up += 1
                peak = up             # 记录上升长度（山峰高度）
                down = 0              # 重置下降段
                min_number_of_candies += 1 + up   # 当前孩子糖果 = 上升长度 + 1

            elif ratings[i] == ratings[i - 1]:
                # 持平：重置所有计数
                up = down = peak = 0
                min_number_of_candies += 1        # 每人至少 1 颗糖

            else:
                # 下降趋势：右边孩子评分更低
                up = 0
                down += 1
                # 若下降没超过上升峰值，说明峰顶已够高，需要减去重复的那 1
                if peak >= down:
                    min_number_of_candies += down
                else:
                    # 下降更长时，峰顶必须再高 1 才压得住右边
                    min_number_of_candies += down + 1

        return min_number_of_candies
