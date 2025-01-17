---
Creation Time: Thursday, November 7th 2024
Modified Time: Friday, November 8th 2024
---

Given an integer array `nums` that may contain duplicates, return _all possible_ 

_subsets_

 _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

```java
public void subsetsDuplicate(int[] nums, List<List<Integer>> res, int index, List<Integer> currRes) {

	res.add(new ArrayList<>(currRes));
	
	for(int i=index; i<nums.length;i++){
	
		if(i!=index && nums[i]==nums[i-1])
	
		continue;
	
		currRes.add(nums[i]);
		
		subsetsDuplicate(nums, res, i+1, currRes);
		
		currRes.remove(currRes.size()-1);

}
	
public List<List<Integer>> subsetsWithDup(int[] nums) {

	List<List<Integer>> res = new ArrayList<>();
	
	Arrays.sort(nums);
	
	subsetsDuplicate(nums, res, 0, new ArrayList<>());
	
	return res;

}

}
```