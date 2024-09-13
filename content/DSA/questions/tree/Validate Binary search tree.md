---
Creation Time: Thursday, September 12th 2024
Modified Time: Thursday, September 12th 2024
---
```java
public boolean validate(TreeNode root, long min, long max){

	if(root == null)
	
		return true;

	if(root.val <= min || root.val >= max){
	
		return false;

	}

	return validate(root.left, min, root.val) && validate(root.right, root.val, max);

}

public boolean isValidBST(TreeNode root) {

	return validate(root, Long.MIN_VALUE, Long.MAX_VALUE );

}
```