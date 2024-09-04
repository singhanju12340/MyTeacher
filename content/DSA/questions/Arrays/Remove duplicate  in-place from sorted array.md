---
Creation Time: Tuesday, August 13th 2024
Modified Time: Tuesday, August 13th 2024
---
1. Use Set
	1. Insert in Set: NLog(N) + Time O(N)
	2. Space O(N)
2. 2 Pointer
	1. ``` java
			fun(){
			int fP = 0;
			int sP = 1;
				for(int sP=1, sP<n;sP++){
					if(a[fP] != a[sP])
					{
						a[fP+1] = a[sP];
						fP++;
					}	
				}
				return fP+1;
			}
```
Time: O(N)
Space: O(1)