# Island Perimeter

## Problem Statement

You are given `grid`, an `m x n` 2-D integer array where:

- `1` represents **land**
- `0` represents **water**

The grid represents a map of an island (or islands) — there is exactly **one island**, and it doesn't have any lakes (water inside the island that isn't connected to the surrounding water).

Each cell is a square with side length `1`. The island is formed by connecting adjacent land cells **horizontally/vertically** (not diagonally).

Return the **perimeter** of the island.

---

## Example

### Input

```text
grid = [
    [0, 1, 0, 0],
    [1, 1, 1, 0],
    [0, 1, 0, 0],
    [1, 1, 0, 0]
]
```

### Output

```text
16
```

---

## Approach

Instead of tracing the outline of the island, we can count the perimeter **cell by cell**.

### Key Idea

Every single land cell contributes **4** sides to the perimeter — unless one of its 4 neighbors is also land, in which case that shared edge is **internal** and should not be counted.

So for every land cell:

- Start by adding `4` to the perimeter.
- For each of its 4 neighbors (up, down, left, right) that is also land, subtract `1` (since that edge is shared, not a boundary edge).

### Steps

1. Loop through every cell `(i, j)` in the grid.
2. If `grid[i][j] == 1`:
   - Add `4` to `perimeter`.
   - Check the 4 neighboring cells (with boundary checks).
   - For every neighbor that is also `1`, subtract `1` from `perimeter`.
3. Return the final `perimeter`.

This works because every internal shared edge between two land cells gets counted **once from each side** — so it's correctly removed from both cells' contribution of 4.

---

## Important Logic

### 1. Base Contribution

```java
if (grid[i][j] == 1)
{
    perimeter += 4;
    ...
}
```

Every land cell starts out assuming all 4 of its sides are exposed to water (i.e., part of the perimeter).

### 2. Up Neighbor Check

```java
if (i > 0 && grid[i-1][j] == 1)
{
    c++;
}
```

`i > 0` ensures we don't go out of bounds on the top edge. If the cell above is also land, the shared edge isn't part of the perimeter.

### 3. Down Neighbor Check

```java
if (i < m-1 && grid[i+1][j] == 1)
{
    c++;
}
```

`i < m-1` ensures we don't go past the last row.

### 4. Left Neighbor Check

```java
if (j > 0 && grid[i][j-1] == 1)
{
    c++;
}
```

`j > 0` ensures we don't go out of bounds on the left edge.

### 5. Right Neighbor Check

```java
if (j < n-1 && grid[i][j+1] == 1)
{
    c++;
}
```

`j < n-1` ensures we don't go past the last column.

### 6. Subtract Shared Edges

```java
perimeter -= c;
```

`c` holds the count of land neighbors for the current cell. Each land neighbor means one side of this cell is touching another land cell instead of water, so it's removed from the perimeter total.

---

## Complete Java Code

```java
class Solution
{
    public int islandPerimeter(int[][] grid)
    {
        int m = grid.length;
        int n = grid[0].length;
        int perimeter = 0;

        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (grid[i][j] == 1)
                {
                    perimeter += 4;
                    int c = 0;

                    // up neighbor
                    if (i > 0 && grid[i-1][j] == 1)
                    {
                        c++;
                    }
                    // down neighbor
                    if (i < m-1 && grid[i+1][j] == 1)
                    {
                        c++;
                    }
                    // left neighbor
                    if (j > 0 && grid[i][j-1] == 1)
                    {
                        c++;
                    }
                    // right neighbor
                    if (j < n-1 && grid[i][j+1] == 1)
                    {
                        c++;
                    }

                    perimeter -= c;
                }
            }
        }

        return perimeter;
    }
}
```

---

## Dry Run

Consider:

```text
0 1 0 0
1 1 1 0
0 1 0 0
1 1 0 0
```

We scan every cell and only process land (`1`) cells.

| Cell (i,j) | Base | Land Neighbors       | Contribution |
|------------|------|-----------------------|--------------|
| (0,1)      | 4    | 1 (down)               | 3            |
| (1,0)      | 4    | 1 (right)              | 3            |
| (1,1)      | 4    | 3 (up, left, right)    | 1            |
| (1,2)      | 4    | 1 (left)               | 3            |
| (2,1)      | 4    | 2 (up, down)           | 2            |
| (3,0)      | 4    | 1 (right)              | 3            |
| (3,1)      | 4    | 2 (up, left)           | 1            |

Adding up all contributions:

```
3 + 3 + 1 + 3 + 2 + 3 + 1 = 16
```

**Output: `16`** — matches the expected result.

---

## Why Doesn't This Need DFS/BFS?

Unlike problems such as **Flood Fill** or **Number of Islands**, we don't actually need to traverse the island as a connected shape here.

The perimeter only depends on **local, cell-level information** — each land cell only needs to know about its immediate 4 neighbors to determine how many of its sides are exposed. There's no need to track visited cells or explore connectivity, since we're not identifying regions — we're just summing up boundary edges.

This makes the single-pass nested loop sufficient and avoids the overhead of a recursive/stack-based traversal.

---

## Complexity Analysis

Let:

```
m = number of rows
n = number of columns
```

### Time Complexity

```
O(m × n)
```

**Why?**

We visit every cell in the grid exactly once, and for each land cell we do a constant amount of work (checking 4 neighbors). This gives O(m × n) overall.

### Space Complexity

```
O(1)
```

**Why?**

No auxiliary data structures (like a `visited` array or recursion stack) are used — only a few integer variables (`perimeter`, `c`, `m`, `n`) are maintained, regardless of the size of the grid. This gives constant extra space.
