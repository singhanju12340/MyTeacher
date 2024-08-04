> arr = {1, 2, 3}
> using coins only once
> target = 7
> return number of coins needed 

```java

fun( int[] arr, int target, int index){
	if(index == 0){
		if( arr[0] == target ){
			return 1;
		}else return 0;	
	}
		
		if(arr[index] >= target)
	int take = 1 + fun(arr, target-arr[index], index-1);
	int notTake = 0 + fun(arr, target, index-1);
	
	return Math.min(take, nonTake);
}
```

``` java
dp[arr.length][target+1]

funTab(){
	for(int i=0;i<target;i++){
		dp[0][i] = 0;
	}
	dp[0][target] = 1;
	
	for(int i=1; i < arr.length ; i++){
		for(int j=0; j <= target; j++){
				if(arr[i] >= j)
				int take = 1 + dp[i-1][j-1]; //target-arr[index], index-1];
				int notTake = 0 + dp[i-1][j]; //[target][index-1];
				dp[i][j] = min(take, notTake);
		}
	}
}
	
```


