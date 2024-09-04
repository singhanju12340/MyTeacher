---
Creation Time: Thursday, August 22nd 2024
Modified Time: Thursday, August 22nd 2024
---
Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

```java
public int maxDepth(TreeNode root) {

	if(root == null)
	
	return 0;
	
	return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));

}
```