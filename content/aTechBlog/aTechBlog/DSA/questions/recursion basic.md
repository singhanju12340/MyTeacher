print 1 to n
print n to 1
print 1 to n without using i+1
print n to 1 without using i-1
sum of first n numbers
```Botton up, i=1
```sumN(int i, int n){
	if(i==n)
		return i;
	else{
		return i + sumN(i+1, n);
	}
}

Top down, i=N, sum=0

sumN(int i, int sum){
	if(i<1){
			print(sum);
			return;
	}
	sumN(i-1, sum+i);

}

```

**Reverse an array**
|1|2|3|4|5|6|7|8|9
-- 
```
start=0, end=n-1
reverse( int start, int end , arr){
	if( start >= end)
		return;
	reverse( start+1, end-1 , arr);
		int temp = arr[start];
		arr[start] = arr[end];
		arr[end]=temp;	
}
```
Using only extra single variables
```
start=0
reverse( int start, arr){
	if( start >= arr.size()/2)
		return;
	reverse( start+1 , arr);
		int temp = arr[start];
		arr[start] = arr[arr.size-start-1];
		arr[arr.size-start-1]=temp;	
}
```

