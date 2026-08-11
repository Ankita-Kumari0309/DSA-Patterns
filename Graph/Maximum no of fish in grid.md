## 2658. Maximum Number of Fish in a Grid

Link - 

```
https://leetcode.com/problems/maximum-number-of-fish-in-a-grid/description/
```

```
You are given a 0-indexed 2D matrix grid of size m x n, where (r, c) represents:

A land cell if grid[r][c] = 0, or
A water cell containing grid[r][c] fish, if grid[r][c] > 0.
A fisher can start at any water cell (r, c) and can do the following operations any number of times:

Catch all the fish at cell (r, c), or
Move to any adjacent water cell.
Return the maximum number of fish the fisher can catch if he chooses his starting cell optimally, or 0 if no water cell exists.

An adjacent cell of the cell (r, c), is one of the cells (r, c + 1), (r, c - 1), (r + 1, c) or (r - 1, c) if it exists.

 ```



```
class Solution 
{
    int m,n;
    public int findMaxFish(int[][] grid) 
    {
        m = grid.length;
        n = grid[0].length;
        int[][] visited = new int[m][n];
        
        int maxi = 0;
        for(int i = 0;i<m;i++)
        {
            for(int j = 0;j<n;j++)
            {
                if(grid[i][j] > 0 && visited[i][j] == 0)
                {
                    int fish = bfs(grid,visited,i,j);
                    maxi = Math.max(maxi,fish);
                }
            }
        }
        
        return maxi;
    }
    int bfs(int[][] grid,int[][] visited,int i,int j)
    {
        visited[i][j] = 1;
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[]{i,j});
        int fish = 0;
        int[][] dir = {{-1,0},{0,-1},{0,1},{1,0}};
        while(!q.isEmpty())
        {
            int[] node = q.poll();
            int r = node[0];
            int c = node[1];
            fish += grid[r][c]; 
            for(int[] d : dir)
            {
                int nr = r+d[0];
                int nc = c+d[1];
                if(nr >= 0 && nr <m && nc >= 0 && nc <n && grid[nr][nc] > 0 && visited[nr][nc] == 0)
                {
                    q.offer(new int[]{nr,nc});
                    visited[nr][nc] = 1;
                }
            }
        }
        return fish;
    }

```

}
