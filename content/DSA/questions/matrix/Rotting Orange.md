---
Creation Time: Monday, September 16th 2024
Modified Time: Monday, September 16th 2024
---
You are given an `m x n` `grid` where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.


**Input:** grid = [[2,1,1],[1,1,0],[0,1,1]]
**Output:** 4

**Example 2:**

**Input:** grid = [[2,1,1],[0,1,1],[1,0,1]]
**Output:** -1

```java
	class Node{  
    int r;  
    int c;  
    int sec;  
    Node(int row, int col, int sec){  
        r = row;  
        c = col;  
        this.sec = sec;  
    }  
}

class Solution {
	public int orangesRotting(int[][] grid) {  
        int[][] visited = new int[grid.length][grid[0].length];  
        int minRes = 0;  
        Queue<Node> qu = new LinkedList<Node>();  
        int freshCount =0;  
        for(int i=0;i<grid.length;i++){  
            for(int j=0;j<grid[0].length;j++){  
                if(grid[i][j] == 1){  
                    freshCount++;  
                }  
                else if( grid[i][j] == 2){  
                    qu.add(new Node(i, j, 0));  
                    visited[i][j] = 1;  
                }  
            }  
        }  
        // minRes = bfs(visited, grid, freshCount, qu);  
  
        int[] dirX = new int[]{-1,1,0,0};  
        int[] dirY = new int[]{0,0,-1,1};  
        while(!qu.isEmpty()){  
            Node curr = qu.poll();  
            int time = curr.sec;  
            minRes = Math.max(minRes, time);  
  
            for(int i=0;i<4;i++){  
                int newX = curr.r + dirX[i];  
                int newY = curr.c + dirY[i];  
                if(newX>=0 && newY>=0 && newX<grid.length && newY<grid[0].length && visited[newX][newY] == 0 &&  
                        grid[newX][newY] == 1  
                ){  
                    freshCount--;  
                    visited[newX][newY] = 1;  
                    qu.add(new Node(newX, newY, time+1));  
                }  
            }  
  
        }  
  
  
        if(freshCount>0)  
            return -1;  
        return minRes;  
  
	}
	

  public static void main(String[] args) {  
    int[][] grid = new int[][]{{2,1,1},{1,1,0},{0,1,1}};  
    System.out.println(new rottingOrange().orangesRotting(grid));  
}
  

}
```