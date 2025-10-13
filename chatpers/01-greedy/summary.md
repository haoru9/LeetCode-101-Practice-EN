# LeetCode 101 · Chapter 1 贪心算法（Greedy Algorithms）

---

## 一、贪心算法的本质  

贪心算法的核心思想是：在问题求解过程中，每一步都选择当前看来“最优”的决策（局部最优），希望最终能得到全局最优解。  
使用贪心算法的前提通常是问题具有两个性质：  
1. **贪心选择性质（Greedy Choice Property）**：局部最优解能扩展为全局最优解。  
2. **最优子结构（Optimal Substructure）**：问题的最优解包含子问题的最优解。  
验证贪心是否可行时，常用的数学方法包括：交换论证、反证法、或数学归纳法。  
与动态规划（DP）相比，贪心算法不需要穷举所有状态，它更快、更直接，但也更依赖于问题本身结构。

The core idea of the **Greedy Algorithm** is to make the *best possible decision at each step*—a **locally optimal choice**—in the hope that this will lead to a **globally optimal solution**.  
A problem is suitable for greedy methods when it satisfies:  
1. **Greedy Choice Property** – A local optimum can be part of the global optimum.  
2. **Optimal Substructure** – The global optimum can be built from optimal solutions of subproblems.  
To prove a greedy approach works, we often use **exchange arguments**, **proof by contradiction**, or **mathematical induction**.  
Compared to **Dynamic Programming (DP)**, greedy algorithms are faster and simpler but rely heavily on problem structure and correctness proof.

---

## 二、贪心的常见套路  

贪心算法常见的应用模式包括：  
1. **排序 + 双指针**：对“需求”和“资源”排序后逐步匹配（如分配饼干问题）。  
2. **按结束时间排序**：解决区间调度、去重叠区间类问题。  
3. **左右两次扫描**：当每个元素与左右邻居有关（如发糖果问题）。  
4. **优先队列（堆）**：每次取当前最关键的最大或最小值（如合并石头问题）。  
5. **计数或位置记录**：用出现频次或首末位置辅助选择（如 Partition Labels）。  
6. **栈结构**：用于构造字典序最小或保持单调的结构。  

Common greedy patterns include:  
1. **Sorting + Two Pointers** – Sort demands and resources, then match step by step (e.g., *Assign Cookies*).  
2. **Sorting by End Time** – For scheduling or interval problems (e.g., *Non-overlapping Intervals*).  
3. **Two-pass Scanning** – When each element depends on its neighbors (e.g., *Candy* problem).  
4. **Priority Queue (Heap)** – Always pick the current most important or smallest/largest element.  
5. **Counting / Position Tracking** – Use frequency or last occurrence positions (e.g., *Partition Labels*).  
6. **Stack-based Greedy** – Maintain monotonic or lexicographically minimal structures.

---

## 三、做题通用流程  

解决贪心问题的一般步骤：  
1. 明确每一步的**贪心目标**（你在“贪”什么？最小代价？最大收益？）。  
2. 找出比较的**排序标准**（按哪个属性排序？升序还是降序？）。  
3. 选择合适的**数据结构**（双指针、堆、栈、计数数组等）。  
4. 检查**边界条件**（空数组、相等值、重复元素、特殊输入）。  
5. 估算时间复杂度（大多为 `O(n log n)` 或 `O(n)`）。

The general approach to solving greedy problems:  
1. Identify **what to be greedy about**—minimize cost or maximize profit?  
2. Define a **comparison metric**—which attribute to sort by and in what order.  
3. Choose the **right data structure**—two pointers, heap, stack, or counters.  
4. Verify **edge cases**—empty input, duplicates, equal values, etc.  
5. Analyze complexity—usually `O(n log n)` (sorting) or `O(n)` (linear scanning).

---

## 四、典型例题小结  

### 1. 455. 分配饼干（Assign Cookies）

- **思路**：先满足饥饿度最小的孩子，用最小能满足的饼干。  
- **实现**：`g`、`s` 排序，双指针匹配。  
- **复杂度**：`O(n log n)`。  
- **要点**：先小后大，局部最优带来整体最优。

- **Idea:** Always feed the least hungry child first with the smallest cookie that satisfies them.  
- **Implementation:** Sort both arrays (`g` and `s`), use two pointers to match.  
- **Complexity:** `O(n log n)` (due to sorting).  
- **Key Insight:** Prioritize smaller needs first to avoid wasting larger resources.

---

### 2. 135. 发糖果（Candy）

- **思路**：每个孩子至少 1 颗糖。评分高的孩子糖要比相邻低分的多。  
  - 从左到右：若右边评分高，则糖数 +1；  
  - 从右到左：若左边评分高，则左侧糖 = max(左, 右+1)。  
- **复杂度**：`O(n)`。  
- **要点**：两次遍历，左右都考虑到。

- **Idea:** Each child must have at least one candy, and those with higher ratings than neighbors get more.  
  - First pass left → right: if `rating[i] > rating[i-1]`, `candies[i] = candies[i-1] + 1`.  
  - Second pass right → left: adjust to ensure right neighbor constraints.  
- **Complexity:** `O(n)` time.  
- **Key Insight:** Two passes ensure fairness from both sides.

---

### 3. 435. 区间问题（Non-overlapping Intervals）

- **思路**：要删除最少区间，使剩下的不重叠。  
  - 按“结束时间”升序排序，每次选最早结束且不冲突的区间。  
  - 若冲突则删除结束更晚的。  
- **复杂度**：`O(n log n)`。  
- **要点**：区间调度的经典贪心策略。

- **Idea:** Remove the fewest intervals to make the rest non-overlapping.  
  - Sort intervals by **end time** ascending.  
  - Always keep the earliest-ending interval that doesn’t overlap.  
- **Complexity:** `O(n log n)`.  
- **Key Insight:** Choosing intervals that finish earliest leaves more space for others.

---

## 五、延伸题型  

这些是贪心思想的常见变体：  
- **605. Can Place Flowers**：遇到可种位置就种，能种就种。  
- **452. Minimum Number of Arrows to Burst Balloons**：按右端点排序，用最少箭射穿所有区间。  
- **763. Partition Labels**：根据字符的最远出现位置切分。  
- **122. Best Time to Buy and Sell Stock II**：每次上涨就交易一次。  
- **406. Queue Reconstruction by Height**：按身高降序、`k` 升序插入。  
- **665. Non-decreasing Array**：一次修改，局部修正保证非降。

These are common variations of greedy thinking:  
- **605. Can Place Flowers:** Plant whenever possible — local greedy choice.  
- **452. Minimum Arrows to Burst Balloons:** Sort by right endpoint; shoot one arrow per overlapping group.  
- **763. Partition Labels:** Split string based on last occurrence of each character.  
- **122. Best Time to Buy and Sell Stock II:** Sum all profit increases.  
- **406. Queue Reconstruction by Height:** Sort by height descending, `k` ascending, insert in order.  
- **665. Non-decreasing Array:** Fix at most one violation locally to maintain non-decreasing order.

---

## 六、常见坑与检查清单  

- 排序方向错误。  
- 等号边界误用（`<` vs `<=`）。  
- 区间定义模糊（端点是否重叠）。  
- 贪心目标不清。  
- 忽略了数据结构选择。  

Common pitfalls include:  
- Wrong sorting order.  
- Incorrect boundary handling (`<` vs `<=`).  
- Misunderstanding of interval overlap rules.  
- Undefined greedy metric (what are we optimizing?).  
- Poor choice of supporting data structure.

---

## 七、复杂度与模板思维  

- 贪心题大多复杂度在 `O(n log n)`（排序）或 `O(n)`（线扫）。  
- 模板式思维：  
  - 匹配类：排序 + 双指针；  
  - 区间类：按右端点排序；  
  - 相邻约束：双向扫描；  
  - 取最值类：堆；  
  - 出现位置类：计数或索引。  

Most greedy problems run in `O(n log n)` (sorting) or `O(n)` (scanning).  
Template mindset:  
  - **Matching:** sort + two pointers.  
  - **Intervals:** sort by end time.  
  - **Neighbor Constraints:** two-pass scan.  
  - **Max/Min Selection:** use a heap.  
  - **Position Tracking:** count or record indices.

