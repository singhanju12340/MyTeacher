3 |1|2|4|1|5|2|6|4
--
![[Screenshot 2024-06-19 at 8.22.06 PM.png]]
```
merge(int low, int mid, int height, res[]){
i=low;
j=mid+1;
int k = 0;
	while(i<= mid || j <= heigh){
		if(arr[i] < arr[j])
			res[k++] = arr[i++];
		else
			res[k++] = arr[j++];
	}
	while(i<= mid){
		res[k++] = arrL[i++];
	}
	while(j< height){
		res[k++] = arrR[j++];
	}
}


mergeSort(arr, low, hight){
	if(low>=heigh){
		return;
	}
	int mid = (low+height)/2;
	mergeSort(arr, low, mid);
	mergeSort(arr, mid+1, heigh);
	merge(arr, low, mid, height);
	
}
```