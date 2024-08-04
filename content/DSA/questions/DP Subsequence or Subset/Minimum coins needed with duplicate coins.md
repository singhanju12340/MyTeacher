 arr = {1, 2, 3}

> target = 7

```java
fun( int[] arr, int target, int index){
	if(index == 0){
		if( target%arr[0] ==  0){
			return target/arr[0];
		}else return max;	
	}
		
		if(arr[index] >= target)
	int take = 1 + fun(arr, target-arr[index], index);
	int notTake = 0 + fun(arr, target, index-1);
	
	return Math.min(take, nonTake);
}
```
