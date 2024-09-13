---
Creation Time: Thursday, September 12th 2024
Modified Time: Thursday, September 12th 2024
---
Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

**Input:** nums = [1,2,3,4]
**Output:** [24,12,8,6]

**Example 2:**

**Input:** nums = [-1,1,0,-3,3]
**Output:** [0,0,9,0,0]


```java 
public int[] productExceptSelf(int[] nums) {

int[] prefix = new int[nums.length];

prefix[0]=1;

int[] suffix = new int[nums.length];

suffix[nums.length-1]=1;

  

for(int i=1;i<nums.length;i++){

prefix[i] = prefix[i-1]* nums[i-1];

suffix[nums.length-1-i] = suffix[nums.length-i] * nums[nums.length-i];

}

  

for(int i=0;i<nums.length;i++){

prefix[i] = prefix[i]*suffix[i];

}

  

return prefix;

}
```