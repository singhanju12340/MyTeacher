---
Creation Time: Wednesday, November 6th 2024
Modified Time: Wednesday, November 6th 2024
---
You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:

0 <= j <= nums[i] and  
i + j < n  
Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

Example 1:

Input: nums = [2,3,1,1,4]  
Output: 2  
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

Example 2:

Input: nums = [2,3,0,1,4]  
Output: 2

```java
fun(int[] nums, int[] dp, integer i, int res){
	if( i == nums.size()-1)
		return;
		if(dp[i] != 0)
			return dp[i];
		int take = 0;
		if(res + nums[i] <= n-1)
			{
				take = fun(nums, dp, i+1, res+nums[i]);
			}
		int notTake =  fun(nums, dp, i+1, res);
	dp[i] = Math.min(take, notTake);
	return dp[i];
}
```

```java

class Solution {  
    public int jump(int[] nums) {  
        int n = nums.length;  
        int[] dp = new int[n];  
        Arrays.fill(dp, Integer.MAX_VALUE);  
        dp[0] = 0;  
  
        for (int i = 1; i < n; i++) {  
            for (int j = 0; j < i; j++) {  
                if (j + nums[j] >= i && jump[j] != Integer.MAX_VALUE) {  
                    dp[i] = Math.min(dp[i], dp[j] + 1);  
                }  
            }  
        }  
  
        return dp[n- 1];  
    }  
}
```