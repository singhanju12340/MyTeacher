---
Creation Time: Friday, July 19th 2024
Modified Time: Thursday, November 7th 2024
---
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

```java
public int climbStairs(int n) {

	int[] res= new int[n+1];
	res[0]= 1; // count to reach to stairs 0 : already at step 1, no need to jump
	res[1]= 1; // count to reach to stairs 1 : 1 way only from stairs 0 by taking 1 jump

//res[2]= 2; // count to reach to stairs 2 : 1 way from step 1 y taking 1 jump and 1 way from step 0 by taking 2 jump
//res[3] = 3; count to reach stair 3:
//can reach from 1 by taking 2 jump, can reach to 3 from step 2 by taking 1 jump,
// res = sum of ways from 1st + 2nd stairs
	for(int i=2;i<=n;i++){
		res[i] = res[i-1] + res[i-2];
	}
	return res[n];

}
```