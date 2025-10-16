# 455. Assign Cookies  
**Tags:** Greedy, Sorting, Two Pointers  

---

## Problem Description  

Assume you are an awesome parent and want to give your children some cookies.  
But you should give each child at most one cookie.

Each child `i` has a greed factor `g[i]`, which is the minimum size of a cookie that the child will be content with;  
and each cookie `j` has a size `s[j]`.  
If `s[j] >= g[i]`, we can assign the cookie `j` to the child `i`,  
and the child `i` will be content.  

Your goal is to maximize the number of your content children and output the maximum number.

---

## 中文题意  

你是一位好家长，想给孩子们发饼干。  
每个孩子有一个**饥饿度** `g[i]`，每块饼干有一个**大小** `s[j]`。  
只有当 `s[j] >= g[i]` 时，这个孩子才会被满足。  
每个孩子最多得到一块饼干。  

求最多能满足多少个孩子。

---

## Example 1  

**Input:**  
`g = [1,2,3], s = [1,1]`  
**Output:**  
`1`  

**Explanation:**  
You have 3 children and 2 cookies.  
The greed factors of children are `1, 2, 3`.  
Both cookies are size 1, so only the first child can be satisfied.  
Hence the output is `1`.

---

## Example 2  

**Input:**  
`g = [1,2], s = [1,2,3]`  
**Output:**  
`2`  

**Explanation:**  
You have 2 children and 3 cookies.  
Their greed factors are `1, 2`, and cookie sizes are enough for both.  
Hence the output is `2`.

---

## Constraints  

- `1 <= g.length <= 3 * 10^4`  
- `0 <= s.length <= 3 * 10^4`  
- `1 <= g[i], s[j] <= 2^31 - 1`

---

## Solution (Greedy Approach)  

**Algorithm Idea:**  
1. Sort both arrays `g` (children’s greed) and `s` (cookies’ size).  
2. Use **two pointers** `i` and `j` to track children and cookies.  
3. If `s[j]` can satisfy `g[i]`, assign and move both pointers.  
4. Otherwise, move cookie pointer forward until found a big enough one.  

**Python Code:**  

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        i = j = 0                    # i child，j cookie
        n, m = len(g), len(s)

        while i < n and j < m:
            if s[j] >= g[i]:         # 这块饼干能满足该孩子
                i += 1               # 满足一个孩子
            j += 1                   # 使用了这块饼干（无论是否满足都要移动）
        return i
