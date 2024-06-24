1|2|4|5
--
res: [ 1, 2,4,5, 3, 5, 6, 7, 12, 9, 11, 7 ]

```
fun (arr[], index, int sum) {

	if(index == arr.length){
		res.add(sum);
		return;
	}
	sum = sum + arr.get(index) ;
		fun(arr, index+1, sum )
	sum = sum-arr.get(index) ;
		fun(arr, index+1, sum);
		
}
```

