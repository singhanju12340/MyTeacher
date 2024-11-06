---
Creation Time: Wednesday, October 30th 2024
Modified Time: Wednesday, October 30th 2024
---
**Example 1:**

**Input:** nums = [2,0,2,1,1,0]
**Output:** [0,0,1,1,2,2]

**Example 2:**

**Input:** nums = [2,0,1]
**Output:** [0,1,2]

``` java
public void sortColors(int[] nums) {  
    int low=0;  
    int mid=0;  
    int high=nums.length-1;  
    while(mid<=high){  
        if(nums[mid] == 0){  
            // mid has 0  
            int lowVal = nums[mid];  
            nums[mid] = nums[low];  
            nums[low] = lowVal;  
            low++;  
            mid++;  
  
        }else if(nums[mid] == 1){  
            //mid has 1  
            mid++;  
        }else{  
            // mid has 2  
            int highVal = nums[mid];  
            nums[mid] = nums[high];  
            nums[high] = highVal;  
            high--;  
        }  
    }  
}
```