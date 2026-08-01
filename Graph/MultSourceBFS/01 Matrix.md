# 🔢 01 Matrix (LeetCode 542)

## Problem Statement

Given an `M × N` binary matrix `mat` consisting of only `0`s and `1`s, return a matrix where each cell contains the **distance to the nearest `0`**.

The distance between two adjacent cells is **1**, and movement is allowed only in **4 directions**:

- Up
- Down
- Left
- Right

---

## Example

### Input

```text
mat =
[
 [0,0,0],
 [0,1,0],
 [0,0,0]
]
```

### Output

```text
[
 [0,0,0],
 [0,1,0],
 [0,0,0]
]
```

---

## Another Example

### Input

```text
mat =
[
 [0,0,0],
 [0,1,0],
 [1,1,1]
]
```

### Output

```text
[
 [0,0,0],
 [0,1,0],
 [1,2,1]
]
```

---

# Approach (Multi-Source BFS)

Instead of running BFS from every `1`, we start BFS simultaneously from **all cells containing `0`**.

Since BFS explores nodes level by level, the first time a `1` is visited, it is guaranteed to have found the **shortest distance** to a `0`.

---

# Algorithm

1. Create a distance matrix and initialize every cell with `-1`.
2. Traverse the matrix.
3. Insert every `0` into the queue.
4. Set the distance of all `0`s to `0`.
5. Perform Multi-Source BFS.
6. Visit all four neighbouring cells.
7. If a neighbour has not been visited (`dist == -1`):
   - Assign its distance as `current distance + 1`.
   - Push it into the queue.
8. Continue until the queue becomes empty.
9. Return the distance matrix.

---

# Java Solution

```java
import java.util.*;

class Main
{
    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);

        System.out.println("Enter no of rows and columns.");
        int m = sc.nextInt();
        int n = sc.nextInt();

        System.out.println("Enter the values in the grid");

        int[][] grid = new int[m][n];

        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                grid[i][j] = sc.nextInt();
            }
        }

        int[][] dist = new int[m][n];

        for(int i = 0; i < m; i++)
        {
            Arrays.fill(dist[i], -1);
        }

        Queue<int[]> q = new LinkedList<>();

        // Add all 0s into the queue
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(grid[i][j] == 0)
                {
                    q.offer(new int[]{i, j});
                    dist[i][j] = 0;
                }
            }
        }

        int[][] dir =
        {
            {-1,0},
            {1,0},
            {0,-1},
            {0,1}
        };

        while(!q.isEmpty())
        {
            int[] node = q.poll();

            int r = node[0];
            int c = node[1];

            for(int[] d : dir)
            {
                int nr = r + d[0];
                int nc = c + d[1];

                if(nr >= 0 && nr < m &&
                   nc >= 0 && nc < n &&
                   dist[nr][nc] == -1)
                {
                    dist[nr][nc] = dist[r][c] + 1;

                    q.offer(new int[]{nr, nc});
                }
            }
        }

        System.out.println("The resultant matrix is:");

        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                System.out.print(dist[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

---

# Dry Run

### Input

```text
0 0 0
0 1 0
0 0 0
```

---

### Initial Queue

```
[(0,0), (0,1), (0,2),
 (1,0),        (1,2),
 (2,0), (2,1), (2,2)]
```

Distance Matrix

```
0 0 0
0 -1 0
0 0 0
```

---

### BFS

Process every `0`.

When processing neighbouring cells,

Only `(1,1)` is unvisited.

```
dist[1][1] = 1
```

Queue

```
[(1,1)]
```

Distance Matrix

```
0 0 0
0 1 0
0 0 0
```

Queue becomes empty.

Final answer

```
0 0 0
0 1 0
0 0 0
```

---

# Why Multi-Source BFS?

A straightforward approach is:

- Start BFS from every `1`.
- Find the nearest `0`.

If there are `K` ones,

```
Time = O(K × M × N)
```

which can become

```
O((M × N)²)
```

This is inefficient.

Instead,

- Start BFS from every `0`.
- Compute distances for all cells in a **single traversal**.

Hence the optimal solution is obtained.

---

# Time Complexity

### Initial Matrix Traversal

Traverse the matrix once to:

- Find all `0`s.
- Initialize the queue.
- Initialize the distance matrix.

```
O(M × N)
```

---

### Multi-Source BFS

- Every cell is visited only once.
- Every cell checks at most **4 neighbours**.

```
O(M × N)
```

---

### Overall Time Complexity

```
O(M × N)
+
O(M × N)

= O(2 × M × N)

= O(M × N)
```

---

# Space Complexity

### Distance Matrix

```
O(M × N)
```

---

### Queue

In the worst case, every cell can be inserted into the queue.

```
O(M × N)
```

---

### Overall Space Complexity

```
O(M × N)
```

---

# Sample Input

```text
Enter no of rows and columns.
3
3

Enter the values in the grid

0 0 0
0 1 0
0 0 0
```

---

# Sample Output

```text
The resultant matrix is:

0 0 0
0 1 0
0 0 0
```

---

# Key Learning

- This problem is a classic application of **Multi-Source BFS**.
- All `0`s act as the **source nodes**.
- BFS guarantees that the first time a cell is visited, its distance is the **minimum possible distance** from any source.
- Using a `dist[][]` matrix initialized with `-1` helps track unvisited cells and avoids revisiting them.
- Every cell is processed only once, resulting in an optimal **O(M × N)** solution.
```
