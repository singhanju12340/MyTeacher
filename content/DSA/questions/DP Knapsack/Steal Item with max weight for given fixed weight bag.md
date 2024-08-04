**Example**
Fixed size = 7

| bagSize | 3   | 2   | 5   |
| ------- | --- | --- | --- |
| weight  | 30  | 40  | 60  |
| index   | 0   | 1   | 2   |


``` java
void fun(int index, int currentBagSize, int[] weightArr, int[] bagSizeArr ){
	//base case
	if(index == 0){
		if(currentBagSize>=bagSizeArr[0]) return weightArr[0];
		else return 0;
	}

	int take = Math.INT_MIN;

	// itegration
	
	//take only if current value can be picked
	if(currentBagSize>=bagSizeArr[index]){
		take = weightArr[index] + fun(index-1, currentBagSize-bagSizeArr[index],
		weightArr, bagSizeArr);
	}
	int notTake = 0 + fun(index-1, currentBagSize-bagSizeArr[index], weightArr, bagSizeArr);

	//return
	return Math.max(take, notTake);
}

main(){
	fun(weightArr.length-1, 7)
}

```


### Memorisation Solution
``` java
DP [ weightArr.length ][ FixedSizeBag+1 ]

void fun(int index, int currentBagSize, int[] weightArr, int[] bagSizeArr , int[][] dp){
	//base case
	if(index == 0){
		if(currentBagSize>=bagSizeArr[0]) return weightArr[0];
		else return 0;
	}
	
	if(dp[index][currentBagSize] != -1) return dp[index][currentBagSize];

	int take = Math.INT_MIN;

	// itegration
	
	//take only if current value can be picked
	if(currentBagSize>=bagSizeArr[index]){
		take = weightArr[index] + fun(index-1, currentBagSize-bagSizeArr[index],
		weightArr, bagSizeArr);
	}
	int notTake = 0 + fun(index-1, currentBagSize-bagSizeArr[index], weightArr, bagSizeArr);

	//return
	return dp[index][currentBagSize] = Math.max(take, notTake);
}

main(){
	fun(weightArr.length-1, 7)
}```
	
```


### Tabulation approach

```
DP [ weightArr.length ][ FixedSizeBag+1 ], init DP array with int_min

``` java
fun(int index, int currentBagSize, int[] weightArr, int[] bagSizeArr , int[][] dp)
{
	for(int i=bagSizeArr[0]; i<=currentBagSize; i++ ){
		dp[0][i] = weightArr[0];
	}

	for(int i =1; i< weightArr-1;i++){
		for(int j = 0; j<= currentBagSize; j++){
			if(j >= bagSizeArr[j])
				int take = weightArr[i] + dp[i-1][j-bagSizeArr[j]];
			else take = INT_MIN;
			
			int noTake = 0 + dp[i-1][j];
			dp[i][j] = max( take, noTake );
		}
	}
	return dp[weightArr.length-1][FixedSizeBag];
}
	
```


### Space optimisation

Use prev and current array instead of complete matrix
``` java

	
	prev[weightArr.length];
	curr[weightArr.length];
	
fun(int index, int currentBagSize, int[] weightArr, int[] bagSizeArr , int[][] dp)
{
	for(int i=bagSizeArr[0]; i<=currentBagSize; i++ ){
		//dp[0][i] = weightArr[0];
		prev[i] = weightArr[0];
	}
	
	//

	for(int i =1; i< weightArr-1;i++){
		for(int j = 0; j<= currentBagSize; j++){
			if(j >= bagSizeArr[j])
				//int take = weightArr[i] + dp[i-1][j-bagSizeArr[j]];
				int take = weightArr[i] + prev[j-bagSizeArr[j]];
			else take = INT_MIN;
			
			//int noTake = 0 + dp[i-1][j];
			int noTake = 0 + prev[j];
			//dp[i][j] = max( take, noTake );
			
			curr[j] = max( take, noTake );
		}
		prev = curr;
	}
	return prev[FixedSizeBag];
}
```
