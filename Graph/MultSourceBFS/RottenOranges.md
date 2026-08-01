# 🍊 Rotten Oranges (LeetCode 994)

## Problem Statement

An `M × N` grid is given where each cell can have one of the following values:

- `0` → Empty cell (No orange)
- `1` → Fresh Orange
- `2` → Rotten Orange

Every **1 minute**, a rotten orange can rot all of its **4-directionally adjacent** fresh oranges (up, down, left, right).

Return the **minimum number of minutes** required to rot all the fresh oranges.

If it is **impossible** to rot every fresh orange, return **-1**.

---

## Example

### Input

```text
grid =
[
 [2,1,1],
 [1,1,0],
 [0,1,1]
]
```

### Output

```text
4
```

### Explanation

```
Minute 0
2 1 1
1 1 0
0 1 1

Minute 1
2 2 1
2 1 0
0 1 1

Minute 2
2 2 2
2 2 0
0 1 1

Minute 3
2 2 2
2 2 0
0 2 1

Minute 4
2 2 2
2 2 0
0 2 2
```

All fresh oranges become rotten after **4 minutes**.

---

# Approach (Multi-Source BFS)

Instead of starting BFS from a single source, we start BFS simultaneously from **all initially rotten oranges**.

### Algorithm

1. Traverse the entire grid.
2. Insert every rotten orange (`2`) into the queue.
3. Count the total number of fresh oranges (`1`).
4. Perform Multi-Source BFS.
5. For every rotten orange, rot all adjacent fresh oranges.
6. Mark the fresh orange as rotten immediately to avoid revisiting it.
7. Push the newly rotten orange into the queue.
8. After processing one BFS level, increment the minute count.
9. Continue until the queue becomes empty.
10. If fresh oranges still remain, return `-1`; otherwise, return the total minutes.

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

        Queue<int[]> q = new LinkedList<>();

        int fresh = 0;

        // Store all rotten oranges and count fresh oranges
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(grid[i][j] == 2)
                {
                    q.offer(new int[]{i, j});
                }
                else if(grid[i][j] == 1)
                {
                    fresh++;
                }
            }
        }

        if(fresh == 0)
        {
            System.out.println("The minimum minutes required is 0");
            return;
        }

        int minutes = 0;

        int[][] dir =
        {
            {-1,0},
            {1,0},
            {0,-1},
            {0,1}
        };

        while(!q.isEmpty())
        {
            int size = q.size();

            for(int i = 0; i < size; i++)
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
                       grid[nr][nc] == 1)
                    {
                        grid[nr][nc] = 2;
                        fresh--;

                        q.offer(new int[]{nr, nc});
                    }
                }
            }

            if(!q.isEmpty())
            {
                minutes++;
            }
        }

        if(fresh > 0)
        {
            System.out.println(-1);
            return;
        }

        System.out.println("The minimum minutes required is " + minutes);
    }
}
```

---

# Dry Run

### Input

```text
2 1 1
1 1 0
0 1 1
```

### Initial State

Queue

```
[(0,0)]
```

Fresh Oranges = **6**

Minutes = **0**

---

### Minute 1

Rotten oranges spread to

```
(0,1)
(1,0)
```

Queue

```
[(0,1), (1,0)]
```

Fresh = **4**

---

### Minute 2

New rotten oranges

```
(0,2)
(1,1)
```

Queue

```
[(0,2), (1,1)]
```

Fresh = **2**

---

### Minute 3

New rotten orange

```
(2,1)
```

Queue

```
[(2,1)]
```

Fresh = **1**

---

### Minute 4

New rotten orange

```
(2,2)
```

Queue

```
[(2,2)]
```

Fresh = **0**

Queue becomes empty.

Answer = **4**

---

# Time Complexity

### Initial Grid Traversal

We traverse the entire grid once to:

- Count fresh oranges.
- Insert all rotten oranges into the queue.

**Time:** `O(M × N)`

---

### Multi-Source BFS

- Every cell is inserted into the queue at most once.
- Every orange explores only 4 neighbours.

**Time:** `O(M × N)`

---

### Overall Time Complexity

```
O(M × N) + O(M × N)

= O(2 × M × N)

= O(M × N)
```

---

# Space Complexity

### Queue

In the worst case, every cell may be present in the queue once.

```
O(M × N)
```

### Extra Variables

```
fresh
minutes
direction array
```

require constant space.

```
O(1)
```

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

2 1 1
1 1 0
0 1 1
```

---

# Sample Output

```text
The minimum minutes required is 4
```

---

# Key Learning

- Multi-Source BFS starts BFS from **multiple starting nodes** simultaneously.
- BFS naturally processes nodes **level by level**, where each level represents **1 minute**.
- Mark a fresh orange as rotten immediately after visiting it to avoid duplicate processing.
- Every cell is processed at most once, giving an optimal **O(M × N)** solution.
