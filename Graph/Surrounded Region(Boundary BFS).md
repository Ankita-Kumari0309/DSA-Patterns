## Problem - https://leetcode.com/problems/surrounded-regions/
Boundary BFS - top ,down, left and right BFS

```
class Solution 
{
    int m,n;
    public void solve(char[][] board) 
    {
        m = board.length;
        n = board[0].length;
        Queue<int[]> q = new LinkedList<>();
        //left row
        for(int i = 0;i<m;i++)
        {
            if(board[i][0] == 'O')
            {
                bfs(board,i,0);
            }
        }
        //right row
        for(int i =0;i<m;i++)
        {
            if(board[i][n-1] == 'O')
            {
                bfs(board,i,n-1);
            }
        }
        //top row
        for(int j =0;j<n;j++)
        {
            if(board[0][j] == 'O')
            {
                bfs(board,0,j);
            }
        }
        //last row
        for(int j = 0;j<n;j++)
        {
            if(board[m-1][j] == 'O')
            {
                bfs(board,m-1,j);
            }
        }
        for(int i = 0;i<m;i++)
        {
            for(int j =0;j<n;j++)
            {
                if(board[i][j] == 'O')
                {
                    board[i][j] = 'X';
                }
                else if(board[i][j] == 'S')
                {
                    board[i][j] = 'O';
                }
            }
        }
    }
    void bfs(char[][] board,int r,int c)
    {
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[]{r,c});
        board[r][c] = 'S';//if save in boundary then O not then X
        int[][] dir = {{-1,0},{1,0},{0,-1},{0,1}};
        while(!q.isEmpty())
        {
            int[] node = q.poll();
            int row = node[0];
            int col = node[1];
            for(int[] d:dir)
            {
                int nr = row+d[0];
                int nc = col+d[1];
                if(nr >= 0 && nr < m && nc >= 0 && nc < n && board[nr][nc] == 'O')
                {
                    board[nr][nc] = 'S'; // if 0 and in boundary themn safe
                    q.offer(new int[]{nr,nc});
                }
            }  

        }
    }
}
```
