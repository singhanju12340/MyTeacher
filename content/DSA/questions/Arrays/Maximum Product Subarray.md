---
Creation Time: Thursday, August 22nd 2024
Modified Time: Thursday, August 22nd 2024
---
Given an integer array `nums`, find a subarray that has the largest product, and return _the product_.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [2,3,-2,4]
**Output:** 6
**Explanation:** [2,3] has the largest product 6.

**Example 2:**

**Input:** nums = [-2,0,-1]
**Output:** 0
**Explanation:** The result cannot be 2, because [-2,-1] is not a subarray.

```java
public int maxProduct(int[] nums) {
int max = 0;
int sum=1;
	for(int i = 0; i< nums.length; i++){
		if(nums[i] > 0){
			sum = sum * nums[i];
			max = Math.max(max, sum);
		}else
			sum=1;
		
	}
}
```