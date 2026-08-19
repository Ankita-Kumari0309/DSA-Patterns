# Flood Fill

## Problem Statement

An `m x n` 2-D integer array `image` is given where each cell represents a color.

We are also given:

- `sr` → starting row
- `sc` → starting column
- `color` → new color

Starting from `(sr, sc)`, change the color of the starting cell and all **4-directionally connected cells having the same original color** to the given `color`.

We can move only in:

- Up
- Down
- Left
- Right

Diagonal movement is not allowed.

---

## Example

### Input

```text
image = [
    [1, 1, 1],
    [1, 1, 0],
    [1, 0, 1]
]

sr = 1
sc = 1
color = 2
```

### Output

```text
[
    [2, 2, 2],
    [2, 2, 0],
    [2, 0, 1]
]
```

The `1` at `(2,2)` is not changed because it is not connected to the starting cell through the four allowed directions.

---

## Approach

We use **DFS (Depth First Search)** to explore all connected cells.

### Steps

1. Store the original color of the starting cell.
2. If the original color is already equal to the new color, return the image.
3. Start DFS from `(sr, sc)`.
4. For every cell:
   - Check whether it is outside the matrix.
   - Check whether its color is different from the original color.
   - If either condition is true, stop.
   - Change the current cell to the new color.
   - Recursively visit the four neighboring cells:
     - Up
     - Down
     - Left
     - Right
5. Return the modified image.

---

## Important Logic

### 1. Store Original Color

```java
int originalColor = image[sr][sc];
```

We need the original color because `color` represents the new color.

For example:

```
originalColor = 1
newColor = 2
```

We should change only the connected cells that originally contained `1`.

### 2. Same Color Check

If:

```
originalColor == color
```

there is nothing to change.

```java
if (originalColor == color)
{
    return image;
}
```

This also prevents unnecessary recursion.

### 3. Boundary Check

```java
if (sr < 0 || sc < 0 || sr >= m || sc >= n)
{
    return;
}
```

This stops DFS when the current position goes outside the matrix.

We use `||` because if any one condition is invalid, we must stop.

### 4. Original Color Check

```java
if (image[sr][sc] != originalColor)
{
    return;
}
```

If the current cell does not have the original color, it is not part of the region that needs to be filled.

### 5. Change the Color

```java
image[sr][sc] = color;
```

Once the cell is valid, change it to the new color.

### 6. Visit Four Directions

```java
// Up
dfs(image, sr - 1, sc, m, n, color, originalColor);

// Down
dfs(image, sr + 1, sc, m, n, color, originalColor);

// Left
dfs(image, sr, sc - 1, m, n, color, originalColor);

// Right
dfs(image, sr, sc + 1, m, n, color, originalColor);
```

---

## Complete Java Code

```java
class Solution
{
    public int[][] floodFill(int[][] image, int sr, int sc, int color)
    {
        int m = image.length;
        int n = image[0].length;

        // Store the original color
        int originalColor = image[sr][sc];

        // If original color is already the new color
        if (originalColor == color)
        {
            return image;
        }

        // Start DFS
        dfs(image, sr, sc, m, n, color, originalColor);

        return image;
    }

    public void dfs(int[][] image, int sr, int sc,
                     int m, int n,
                     int color, int originalColor)
    {
        // Boundary check
        if (sr < 0 || sc < 0 || sr >= m || sc >= n)
        {
            return;
        }

        // If current cell does not have original color
        if (image[sr][sc] != originalColor)
        {
            return;
        }

        // Change current cell to new color
        image[sr][sc] = color;

        // Up
        dfs(image, sr - 1, sc, m, n, color, originalColor);

        // Down
        dfs(image, sr + 1, sc, m, n, color, originalColor);

        // Left
        dfs(image, sr, sc - 1, m, n, color, originalColor);

        // Right
        dfs(image, sr, sc + 1, m, n, color, originalColor);
    }
}
```

---

## Dry Run

Consider:

```text
1 1 1
1 1 0
1 0 1
```

Starting position:

```
sr = 1
sc = 1
```

New color:

```
color = 2
```

Therefore:

```
originalColor = image[1][1]
              = 1
```

Start DFS from `(1,1)`.

**Step 1**

Current cell:

```
(1,1) = 1
```

Change:

```
1 → 2
```

**Step 2**

Check the four neighbors:

```
        (0,1)
           ↑
           |
(1,0) ← (1,1) → (1,2)
           |
           ↓
        (2,1)
```

Only cells having the original color `1` are processed.

**Step 3**

DFS continues recursively through all connected `1`s.

Final result:

```text
2 2 2
2 2 0
2 0 1
```

The bottom-right `1` remains unchanged because it is not connected to the starting region.

---

## Why Don't We Need a `visited` Array?

Normally, in DFS we may use:

```java
boolean[][] visited;
```

But Flood Fill does not need a separate visited array.

When we visit a cell, we change:

```
originalColor → newColor
```

For example:

```
1 → 2
```

If DFS reaches that cell again:

```java
if (image[sr][sc] != originalColor)
{
    return;
}
```

Since:

```
2 != 1
```

DFS stops.

Therefore, the modified image itself works as the visited information.

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

In the worst case, all cells have the same original color and are connected, so DFS visits every cell. Each cell is processed at most once (visited or skipped in O(1) work), giving a total of O(m × n).

### Space Complexity

```
O(m × n)
```

**Why?**

No extra `visited` array is used, but the recursive DFS call stack can grow as deep as the number of connected cells in the worst case (e.g., a matrix that is entirely one color, visited in a snake-like path). This gives a worst-case recursion depth — and therefore auxiliary space — of O(m × n).
