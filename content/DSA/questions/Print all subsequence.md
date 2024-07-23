

|3|1|2
--

sub sequence can be created with take and do not take decisions

choose 
![[Screenshot 2024-06-19 at 7.15.26 PM.png]]

```
```fun(int i, []){
	if(i>=n){
		print([])
		return;
	}
	[].add([i])
	fun(i+1, []) // take
	[].remove([i])
	fun(i+1, []) // do not take
}
```
``