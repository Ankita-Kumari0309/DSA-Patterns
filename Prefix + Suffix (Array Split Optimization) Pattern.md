# 🧠 LeetCode Pattern Guide — Prefix + Suffix Split Optimization

> **TL;DR**: When a problem asks you to split an array and optimize over both halves — precompute prefix (left) and suffix (right), then sweep once. O(n) time.

---

## 📌 What is This Pattern?

When you need to **split an array at index `i`** and combine something from the left side and right side:

```
split at index i:
  LEFT  → [0  ...  i]       ← prefix[i]
  RIGHT → [i+1 ... n-1]     ← suffix[i+1]

answer = max/min( prefix[i]  op  suffix[i+1] )   for all i
```

You precompute:
- **Prefix array** — aggregates from the left (sum, min, max, product)
- **Suffix array** — aggregates from the right (min, max, product)

Then sweep once to find the best split point.

---

## 🔍 How to Identify It 

### ✅ Use this pattern when you see:
- "Split array into two parts"
- "Left side vs right side"
- Maximize/minimize something using **both halves**
- "For each index `i`, compute..."
- Aggregate (sum / min / max / product) on each side

### ❌ Don't use it when you see:
- Sliding window (continuous subarray that expands/shrinks)
- Need to sort first → use greedy
- Arbitrary subset, not a contiguous split
- Overlapping subproblems → use DP

---

## ⚙️ How to Solve It — Step by Step

**Step 1 — Define what to store**
Decide what prefix holds (sum? min? max? product?) based on what the left side contributes.

**Step 2 — Build prefix array**
Sweep left → right. Each entry aggregates from index `0` to `i`.

**Step 3 — Build suffix array**
Sweep right → left. Init `suffix[n-1] = nums[n-1]` (**NOT** `Integer.MAX_VALUE`!).

**Step 4 — Combine and optimize**
For each `i` from `0` to `n-2`, compute `prefix[i] op suffix[i+1]` and track max/min.

### Java Template

```java
// Use long[] for prefix to avoid overflow!
long[] prefix = new long[n];
prefix[0] = nums[0];
for (int i = 1; i < n; i++)
    prefix[i] = prefix[i-1] + nums[i];  // or min/max/product

// suffix[n-1] = nums[n-1]  ← NOT Integer.MAX_VALUE
int[] suffix = new int[n];
suffix[n-1] = nums[n-1];
for (int i = n-2; i >= 0; i--)
    suffix[i] = Math.min(suffix[i+1], nums[i]);

// ← use suffix[i+1], NOT suffix[i]
long ans = Long.MIN_VALUE;
for (int i = 0; i < n-1; i++)
    ans = Math.max(ans, prefix[i] - suffix[i+1]);
```

---

## 🐛 Common Mistakes to Avoid

| ❌ Wrong | ✅ Correct | Why |
|----------|-----------|-----|
| `suffix[n-1] = Integer.MAX_VALUE` | `suffix[n-1] = nums[n-1]` | Suffix stores actual values, not sentinels |
| `int[] prefix` | `long[] prefix` | Sum can overflow int |
| `prefix[i] - suffix[i]` | `prefix[i] - suffix[i+1]` | Right part starts at i+1, not i |
| `int maxi` | `long maxi` | Result can overflow int |

---

## 📚 10 Practice Problems (All Free on LeetCode)

### 🟢 Beginner — Build the foundation

| # | Problem | Difficulty | What to precompute |
|---|---------|------------|-------------------|
| [#1480](https://leetcode.com/problems/running-sum-of-1d-array/) | Running Sum of 1D Array | Easy | Pure prefix sum |
| [#724](https://leetcode.com/problems/find-pivot-index/) | Find Pivot Index | Easy | prefix == suffix |
| [#1422](https://leetcode.com/problems/maximum-score-after-splitting-a-string/) | Max Score After Splitting a String | Easy | prefix count + suffix count |
| [#121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | Best Time to Buy and Sell Stock | Easy | prefix min |
| [#303](https://leetcode.com/problems/range-sum-query-immutable/) | Range Sum Query – Immutable | Easy | prefix sum foundation |

### 🟡 Intermediate — Apply the pattern with a twist

| # | Problem | Difficulty | What to precompute |
|---|---------|------------|-------------------|
| [#238](https://leetcode.com/problems/product-of-array-except-self/) | Product of Array Except Self | Medium | prefix product × suffix product |
| [#2256](https://leetcode.com/problems/minimum-average-difference/) | Minimum Average Difference | Medium | prefix sum split |
| [#2016](https://leetcode.com/problems/maximum-difference-between-increasing-elements/) | Max Difference (Increasing Elements) | Easy | prefix min + sweep |
| [#1031](https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/) | Max Sum of Two Non-Overlapping Subarrays | Medium | prefix max + best left window |

### 🔴 Advanced — Prefix + Suffix as a building block

| # | Problem | Difficulty | What to precompute |
|---|---------|------------|-------------------|
| [#42](https://leetcode.com/problems/trapping-rain-water/) | Trapping Rain Water | Hard | prefix max + suffix max |

---

## 🗺️ Related Patterns — What to Learn Next

| Pattern | When to use | Key signal words |
|---------|------------|-----------------|
| **Sliding Window** | Contiguous subarray with a condition | "longest / shortest subarray with..." |
| **Binary Search on Answer** | Minimize max OR maximize min | "minimum possible", "feasible?" |
| **Greedy + Sort** | Pick locally optimal at each step | "maximize profit", "minimum operations" |
| **Two Pointers** | Sorted array, pair conditions | "two elements", "sum equals target" |
| **1D DP** | Overlapping subproblems, past states matter | "number of ways", "minimum cost path" |

---

## 💡 Key Takeaways

- Always use `long[]` for prefix when dealing with sums — int overflow is a silent killer
- Initialize `suffix[n-1] = nums[n-1]`, never `Integer.MAX_VALUE`
- Always combine `prefix[i]` with `suffix[i+1]`, not `suffix[i]`
- The split pattern is O(n) time and O(n) space — often optimizable to O(1) space
- In contests: **map the problem → known pattern before coding.** Speed comes from pattern recognition.

> Practice these 10 problems in order. By problem 6 you'll start recognizing the pattern instantly.
