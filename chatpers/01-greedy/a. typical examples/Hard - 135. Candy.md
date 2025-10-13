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

## Solution (Two-pass Greedy Approach)  

**Algorithm Idea:**  
1. Give each child 1 candy initially.  
2. Traverse from **left → right**:  
   - If `ratings[i] > ratings[i-1]`, then `candies[i] = candies[i-1] + 1`.  
3. Traverse from **right → left**:  
   - If `ratings[i] > ratings[i+1]`, then `candies[i] = max(candies[i], candies[i+1] + 1)`.  
4. Sum up all values in `candies`.

---

### Python Code

```python
def candy(ratings: list[int]) -> int:
    n = len(ratings)
    candies = [1] * n

    # Left to right
    for i in range(1, n):
        if ratings[i] > ratings[i - 1]:
            candies[i] = candies[i - 1] + 1

    # Right to left
    for i in range(n - 2, -1, -1):
        if ratings[i] > ratings[i + 1]:
            candies[i] = max(candies[i], candies[i + 1] + 1)

    return sum(candies)