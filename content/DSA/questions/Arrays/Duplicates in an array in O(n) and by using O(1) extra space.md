---
Creation Time: Thursday, August 22nd 2024
Modified Time: Thursday, August 22nd 2024
---


Last Updated : 16 Jul, 2024

Given an array of n elements containing elements from **0 to n-1**, with any of these numbers appearing any number of times, find these repeating numbers in O(n) and using only constant memory space.

****Input:**** n = 7 , array = {1, 2, 3, 1, 3, 6, 6}  
****Output:**** 1, 3 and 6.  
****Explanation:**** Duplicate element in the array are 1 , 3 and 6  
  
****Input:**** n = 6, array = {5, 3, 1, 3, 5, 5}  
****Output:**** 3 and 5.  
****Explanation:**** Duplicate element in  the array are 3 and 5

IIDEA is if number is repeating in an array mod will always reach to the same index and same index values will be keep on increasing based on the number of times the values are repeating. 

```java
fun(int[] arr){
	for(int i=0;i<arr.length;i++){
		int index = arr[i] % arr.length;
		arr[index] = arr[index] + arr.length;
	}
	
	for(int i=0;i<arr.length;i++){
		
		if(arr[i]/arr.length >=2)
			sout(i);
	}
}
```