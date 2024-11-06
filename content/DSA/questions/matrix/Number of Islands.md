---
Creation Time: Friday, September 13th 2024
Modified Time: Wednesday, October 30th 2024
---
Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:** grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
**Output:** 1

**Input:** grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
**Output:** 3

```java

class GNode {  
    int row;  
    int col;  
  
    GNode(int i, int j) {  
        this.row = i;  
        this.col = j;  
    }  
}  
  
public int numIslands(char[][] grid) {  
    int[][] visited = new int[grid.length][grid[0].length];  
    int count = 0;  
    Queue<GNode> qu = new LinkedList<GNode>();  
    System.out.println("grid : " + grid.length);  
  
    for (int i = 0; i < grid.length; i++) {  
        for (int j = 0; j < grid[0].length; j++) {  
            System.out.println("i : " + i + " j: " + j + "vis:" + visited[i][j]);  
            if (grid[i][j] == '1' && visited[i][j] == 0) {  
                qu.add(new GNode(i, j));  
                bfs(i, j, grid, visited, qu);  
                count++;  
                System.out.println("count : " + count);  
            }  
            System.out.println("countaaa : " + count);  
        }  
    }  
  
    return count;  
}  
  
public void bfs(int row, int col, char[][] grid, int[][] visited, Queue<GNode> qu) {  
  
    while (!qu.isEmpty()) {  
        GNode current = qu.poll();  
        int[] xPath = new int[] { -1, 1, 0, 0 };  
        int[] yPath = new int[] { 0, 0, -1, 1 };  
        for (int x = 0; x < xPath.length; x++) {  
            int newX = current.row + xPath[x];  
            int newY = current.col + yPath[x];  
            if (newX >= 0 && newY >= 0  
                    && newX < grid.length && newY < grid[0].length  
                    && visited[newX][newY] == 0  
                    && grid[newX][newY] == '1') {  
                visited[newX][newY] = 1;  
                qu.add(new GNode(newX, newY));  
            }  
  
        }  
    }  
}
```

