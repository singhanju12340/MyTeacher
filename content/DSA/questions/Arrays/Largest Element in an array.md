---
Creation Time: Tuesday, August 13th 2024
Modified Time: Tuesday, August 13th 2024
---

1. Sort and return last index time complexity: nlog(n), space: O(1) for quick sort, 
2.  O(n), traverse through an array and keep max pointer


## Second Largest Element in an array
1.  Sort and return last index
	1. time complexity: nlog(n) for sorting + O(N)  traverse for 0 to n-2
	2. space: O(1) for quick sort
2. O(n), traverse through an array and keep max pointer
	1. then traverse again and find the second largest. i.e largest
		1. if(a[i] < largest) and a[i]>sLargest
	2. Total time is 2 O(N)
3. ``` java
			if(a.length<2)
				return -1;
			largest = a[0], sLArgest = Math.INT_MIN;
			for(int i=1;i<n;i++){
					if(a[i]>largest){
						sLArgest = largest;
						largest = a[i];
					}
					if(a[i] < largest && a[i]>sLargest){
						sLArgest = a[i];
					}
					
			}
			return sLargest;
```
Space: O(N)



