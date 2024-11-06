---
Creation Time: Thursday, September 19th 2024
Modified Time: Thursday, September 19th 2024
---
`Disjoint Set algorithm` or Union Find is a data structure which helps in solving problems related to connectivity in graph.
The disjoint set helps efficiently check whether adding an edge would form a cycle by determining if two vertices are in the same connected component.

- - **Union-Find role**:
        - `find()`: Check if two vertices belong to the same set (i.e., if they are in the same connected component).
        - `union()`: Merge two sets when an edge is added to the MST.

Uses:
1. Disjoint sets helps in finding the parent
2. Finding the union : can be find by rank or by  size
3.  Connected Components in an Undirected Graph
4. Cycle Detection in an Undirected Graph
5. Union by Rank and Path Compression

graph edges: [[1,2], [2,3], [3,4], [1,4], [1,5]]
number of edges == number of nodes, so there must be cycle present in the graph. 

initially each not is parent of it self, no nodes are connected
step1: create ultimate parent array

Parent Array:

| 1   | 2   | 3   | 4   | 5   |
| --- | --- | --- | --- | --- |
| 1   | 2   | 3   | 4   | 5   |
Rank Array:

| 1   | 2   | 3   | 4   | 5   |
| --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   |



step2: define connection
[1,2]
1 has become parent of 2

| 1   | 2   | 3   | 4   | 5   | step0  | input                                                                                                              |
| --- | --- | --- | --- | --- | ------ | ------------------------------------------------------------------------------------------------------------------ |
| 1   | 1   | 3   | 4   | 5   | step1  | [1,2]                                                                                                              |
| 1   | 1   | 1   | 4   | 5   | step2  | [2,3]:  2 has parent 1, and 2 is connected to 3, so 3 has parent 1                                                 |
| 1   | 1   | 1   | 1   | 5   | step 3 | [3,4]: 3 has parent 1 and 3 is connected to 4, so 4 has parent 1                                                   |
|     |     |     |     |     | step 4 | [1,4]: 1 has parent 1  and 4 also has parent 1, it means they are already connected new connection will make cycle |
```java
public int find(int x) 
{ 
	if (parent[x] != x)
	 {
		  parent[x] = find(parent[x]); // Path compression 
	 } 
	 return parent[x]; 
}
```



```java
// Union by rank 



public void union(int x, int y) 
{ 
	int rootX = find(x); 
	int rootY = find(y); 
	if (rootX != rootY) 
	{ 
		if (rank[rootX] > rank[rootY]) 
		{ 
			parent[rootY] = rootX;
		} else if (rank[rootX] < rank[rootY]) 
		{ 
			parent[rootX] = rootY; 
		} else 
		{
			 parent[rootY] = rootX; rank[rootX]++; 
		 } 
	}
}
```


```java

class DisjointSet { 
	private int[] parent, rank; 
	// Initialize each element as its own set
	 public DisjointSet(int n) { 
		 parent = new int[n];
		rank = new int[n]; 
		for (int i = 0; i < n; i++) 
		{ 
			parent[i] = i;
			 rank[i] = 0; 
		}
	}
```