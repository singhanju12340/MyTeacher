---
Creation Time: Sunday, August 18th 2024
Modified Time: Sunday, August 18th 2024
---
```java
```fun(int[] arr, int k){
		int position = k % arr.length;
		temp[]
		for(int i=0;i<position;i++){
			temp[i] = arr[i];
		}
		for(int i=position;i<arr.length-position;i++){
			arr[i-position] = arr[i];
		}
		
	for(int i=arr.length-position;i<arr.length;i++){
			arr[i] = temp[i-(arr.length)-position];
		}
}