---
Creation Time: Tuesday, September 10th 2024
Modified Time: Thursday, October 10th 2024
Topological sort:
---
`Example : Course Schedule
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.


_Solution_
1. create visited and path visited array of size N
2. create graph in terms of adjacent list from given 2D array
3. check  for each node N in loop, as there can be some node not visited in DFS route of current node.
4. If Node is not visited go and check its DFS
5. DFS:
	1. Mark current node as visited and path visited
	2. loop though all adj list of current node
	3. if its not visited call Dfs for that also
	4.  if adj node is visited or path visited return false
	5. if any Dfs node return false, return false
	6. at end  mark path visited for current node as 0 and return true;

```java

private boolean dfs(int current, ArrayList<Integer>[] gr, Stack<Integer> st, int[] visited, int[] pathVis ){
		visited[current] = 1;
		pathVis[current] = 1;
		ArrayList<Integer> adjs = gr[current];
		for(Integer currAdj : adjs){
			if(visited[currAdj] == 0){
				if(dfs(currAdj, gr, st, visited, pathVis)==false) return false;
			}else if(pathVis[currAdj] == 1){
				return false;
			}
		}
		pathVis[current] = 0;
		return true;
}

  

public boolean canFinish(int numCourses, int[][] prerequisites) {

	Stack<Integer> st = new Stack<>();
	int[] visited = new int[numCourses];
	int[] pathVis = new int[numCourses];
	ArrayList<Integer>[] gr = makeGraph(prerequisites, numCourses);
	for(int i=0;i<numCourses;i++){
		if(visited[i] == 0){
		if(dfs(i, gr,st, visited, pathVis)==false) return false;	
		}
	}
	return true;
}

  

private ArrayList<Integer>[] makeGraph(int[][] prerequisites, int count){

		ArrayList<Integer>[] result = new ArrayList[count];
		
		for(int i=0;i<count;i++){
			result[i] = new ArrayList<Integer>();
		}
		
		for(int[] edge: prerequisites){
			result[edge[0]].add(edge[1]);
		}
		return result;

}
```