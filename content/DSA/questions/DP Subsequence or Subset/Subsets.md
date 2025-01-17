---
Creation Time: Thursday, November 7th 2024
Modified Time: Friday, November 8th 2024
---
Given an integer array `nums` of **unique** elements, return _all possible_ 

_subsets_

 _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Example 2:**

**Input:** nums = [0]
**Output:** [[],[0]]

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of `nums` are **unique**.
-
```java
public void subset(int[] nums){
	List<List<Integer>> res = new ArrayList<Integer>();
	subsetRec(nums, 0, new ArrayList<>(), res);
}

public void subsetRec(int[] nums, int index, List<Integer> currRes, List<List<Integer>> res){
	List<List<Integer>> res = new ArrayList<Integer>();
	for(int i=index; i<nums.length ; i++){
			if(i != index && nums[i] == nums[i-1])
				continue;
			currRes.add(i);
			subsetRec(nums, i+1, currRes, res);
			currRes.remove(currRes.length()-1);
	}
}
```