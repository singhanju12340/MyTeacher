---
Creation Time: Wednesday, October 30th 2024
Modified Time: Wednesday, October 30th 2024
---
There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.


**Example 1:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2], target = 3
**Output:** -1

**Example 3:**

**Input:** nums = [1], target = 0
**Output:** -1


```java
public int search(int[] nums, int target) {  
    int start = 0;  
    int end = nums.length-1;  
    while(start <= end){  
        int mid = (start+end)/2;  
  
        if(nums[mid] == target)  
            return mid;  
  
        else if( nums[mid] >= nums[start]){ // sorted on the left  
            if(nums[start] <= target && nums[mid] >= target){ // target exist in this sorted part  
                end = mid-1;  
            }else{ // target does not exist in sorted part, ignore sorted part  
                start = mid+1;  
            }  
        }else{  
            if(nums[mid] <= target  && nums[end]>=target){ // target exist in right sorted part  
                start = mid+1;  
            }else{ // target does not exist in sorted part right  
                end = mid - 1;  
            }  
        }  
    }  
  
    return -1;  
}
```