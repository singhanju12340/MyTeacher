example: [1, 2, 2, 3]

```
int func(int[] arr, int target,int  index){
	if(target ==0)
		return 1;
%% 	if(index<0)
		return 0; %%
		if(index==0)
		return (arr[0]==target);
		if(target-arr[index]>=0)
	int take =  func(arr, target-arr[index], index-1) 
	int notTake =  fund(arr, target, index-1);
		return take  + notTake;
}
```



memorisation:
```
```dp[size][target+1]=-1

int func(int[] arr, int target,int  index, int[][]dp){
	if(target ==0)
		return 1;
%% 	if(index<0)
		return 0; %%
		if(index==0)
		return (arr[0]==target);
		if(dp[index][target] !=-1)
			return dp[index][target];
		if(target-arr[index]>=0)
	int take =  func(arr, target-arr[index], index-1) 
	int notTake =  fund(arr, target, index-1);
		return dp[index][target] = (take  + notTake);
}

Ans = dp[n-1][target]



````

Tabulation :
```
dp[size][target+1]=0
int func(int[] arr, int target,int  index, int[][]dp){
	for(int i=0;i<arr.length;i++){
		dp[i][0] == 1;
	}

	if(arr[0]<=s) dp[0][arr[0]] = 1;
	
	for(int i=1; i< arr.length;i++){
		for(int j=1;j<target;j++){
				int take = if(arr[i]<=j) dp[i-1][j-arr[j]]
				int notTake =  dp[i-1][j];
				dp[i][j] = take + notTake;
		}
	}
}

Ans = dp[n-1][target]
```

example:
```
```[0,0,1]
how can we solve it

No of zeros=2
ways to represent zerp: {0}, {0}, {0, 0}, {0,0,1}
= 2^n * ans without zero = 2^2 * 1 = 4


change memorisation
		if(index==0)
		return (arr[0]==target);
	to
	if(index == 0){
		if(target ==0 && arr[0]==0) return 2;
		if(target == 0 || arr[0] == target) return 1;
		return 0;
	}

