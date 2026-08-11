## 695. Max Area of Island
```

You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value 1 in the island.

Return the maximum area of an island in grid. If there is no island, return 0.

```

```

class Solution 
{
    int m,n;
    public int maxAreaOfIsland(int[][] grid) 
    {
        m = grid.length;
        n = grid[0].length;
        int[][] visited = new int[m][n];
        //mark 1 in visited if grid cell is visited
        int maxi = 0;
       
        for(int i = 0;i<m;i++)
        {
            for(int j = 0;j<n;j++)
            {
                if(grid[i][j] == 1 && visited[i][j] == 0)
                {
                    int area = bfs(grid,visited,i,j);
                    maxi = Math.max(maxi,area);
                }
            }
            
        }
        return maxi;
    }
    int  bfs(int[][] grid,int[][] visited,int r,int c)
    {
        visited[r][c] = 1;
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[]{r,c});
        int[][] dir = {{-1,0},{0,-1},{0,1},{1,0}};
        int area  = 0;
        while(!q.isEmpty())
        {
            int[] node = q.poll();
            int row = node[0];
            int col = node[1];
            area++;
            for(int[] d:dir)
            {
                int nr = row+d[0];
                int nc = col+d[1];
                
                if(nr >= 0 && nr < m && nc >= 0 && nc <n && grid[nr][nc] == 1 && visited[nr][nc] == 0)
                {
                    visited[nr][nc] = 1;
                    q.offer(new int[]{nr,nc});
                }
            }
        }
        return area;
    }

}

```
