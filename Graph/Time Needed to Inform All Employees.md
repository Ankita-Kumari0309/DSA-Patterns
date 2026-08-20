# Time Needed to Inform All Employees

LeetCode 1376 — Java solution using DFS on the manager hierarchy tree.

## Problem

A company has `n` employees, each with a unique ID from `0` to `n - 1`. The head of the company has ID `headID`.

Every employee has exactly one direct manager, given by the `manager` array (`manager[headID] = -1`). The reporting structure forms a tree.

The head informs their direct subordinates, who inform their subordinates, and so on. Employee `i` takes `informTime[i]` minutes to inform all of their **direct** subordinates (after which those subordinates start informing their own subordinates).

Return the total number of minutes needed until every employee has been informed.

### Examples

**Example 1**
```
Input: n = 1, headID = 0, manager = [-1], informTime = [0]
Output: 0
```
The head is the only employee, so no time is needed.

**Example 2**
```
Input: n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
Output: 1
```
Employee `2` is the direct manager of everyone and takes 1 minute to inform them all.

### Constraints

- `1 <= n <= 10^5`
- `0 <= headID < n`
- `manager.length == n`
- `0 <= manager[i] < n`
- `manager[headID] == -1`
- `informTime.length == n`
- `0 <= informTime[i] <= 1000`
- `informTime[i] == 0` if employee `i` has no subordinates
- It is guaranteed that all employees can be informed

## Approach

1. **Build the tree.** Convert the flat `manager` array into an adjacency list where each manager maps to a list of their direct subordinates.
2. **DFS from the head.** Starting at `headID` with `cumulative = 0`:
   - If the current employee has no subordinates (a leaf), return the accumulated time — this is how long it took the news to reach them.
   - Otherwise, add this employee's `informTime` to the running total, recurse into each subordinate, and return the **maximum** value across all of them (the longest chain determines the total time).
3. The answer is the maximum "time to reach a leaf" over the whole tree, since informing happens in parallel down every branch.

### Complexity

- **Time:** `O(n)` — each employee is visited once.
- **Space:** `O(n)` — for the adjacency list and recursion stack (worst case `O(n)` depth for a skewed tree).

## Code

```java
class Solution
{
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime)
    {
        List<List<Integer>> list = new ArrayList<>();
        for (int i = 0; i < n; i++)
        {
            list.add(new ArrayList<>());
        }
        for (int i = 0; i < n; i++)
        {
            if (manager[i] != -1)
            {
                list.get(manager[i]).add(i);
            }
        }
        return dfs(list, 0, informTime, headID);
    }

    public int dfs(List<List<Integer>> list, int cumulative, int[] informTime, int headID)
    {
        if (list.get(headID).isEmpty()) // leaf node gets the cumulative time
        {
            return cumulative;
        }
        int maxi = 0;
        int newTime = cumulative + informTime[headID];
        for (int c : list.get(headID))
        {
            maxi = Math.max(maxi, dfs(list, newTime, informTime, c));
        }
        return maxi;
    }
}
```

## Usage

```java
Solution sol = new Solution();
int minutes = sol.numOfMinutes(
    6,
    2,
    new int[]{2, 2, -1, 2, 2, 2},
    new int[]{0, 0, 1, 0, 0, 0}
);
System.out.println(minutes); // 1
```

## Notes / Possible Improvements

- **Recursion depth:** For very skewed trees (e.g., a straight chain of `10^5` employees), the recursive DFS could risk a stack overflow. An iterative DFS/BFS with an explicit stack/queue would be safer for worst-case input sizes.
- **Alternative approach:** This could also be solved bottom-up by memoizing, for each employee, the time needed to inform their entire subtree, then taking `informTime[headID's chain]` recursively upward — but the top-down approach here is simpler and equally efficient.
