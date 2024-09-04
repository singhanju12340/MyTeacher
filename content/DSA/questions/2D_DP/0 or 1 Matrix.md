---
Creation Time: Wednesday, September 4th 2024
Modified Time: Wednesday, September 4th 2024
---
`Given an m x n binary matrix mat, return _the distance of the nearest_ 0 _for each cell_.`

`The distance between two adjacent cells is 1.`


```java
class Solution {

  

public static class rowCol{

int x;

int y;

public rowCol(int x,int y){

this.x = x;

this.y = y;

}

}

  

public int[][] updateMatrix(int[][] mat) {

int[][] result = new int[mat.length][mat[0].length];

Queue<rowCol> que = new LinkedList<>();

int n = mat[0].length;

for(int[] curr : result){

Arrays.fill(curr, -1);

}

for(int i=0; i<mat.length; i++){

for(int j=0; j<mat[i].length; j++){

if(mat[i][j] == 0){

result[i][j] = 0;

que.offer(new rowCol(i, j));

}

}

}

  

int[] xDir = {1,-1, 0, 0};

int[] yDir = {0, 0, 1, -1};

  

while(que.size()!=0){

rowCol curr = que.poll();

for(int i=0;i<4;i++){

int nRow = curr.x+xDir[i];

int nCol = curr.y+yDir[i];

System.out.println(curr.x+xDir[i]+":"+ (curr.y+yDir[i]));

if ( nRow < mat.length &&

nRow >=0 &&

nCol < mat[0].length &&

nCol >=0 &&

result[nRow][nCol] == -1)

{

result[curr.x+xDir[i]][curr.y+yDir[i]] = result[curr.x][curr.y]+1;

que.offer(new rowCol(curr.x+xDir[i],curr.y+yDir[i]));

}

}

  

}

  

return result;

}

}
```