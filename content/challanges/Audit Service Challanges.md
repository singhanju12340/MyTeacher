---
Creation Time: Thursday, September 12th 2024
Modified Time: Thursday, September 12th 2024
---
Problems: 
index sizes:
![[Screenshot 2024-09-12 at 10.57.42 AM.png]]Even if your reads are using indexes, even loading index to RAM will take time

Total document currently -> 811057370
storage size: 195GB
Total size: 1.02TB
Index Sie: 50GB


however, on the overall cost, I’m still doing some analysis. Looks like backup costs are very weird for this cluster. I’m still doing the analysis with support. Will let you know of my findings in a week or so. But it may be that this high write in audit DB is posing a cost issue

The backup cost is actually ending up to be 2x the compute cost
backup as in?
	explain backup cost???

Actually it keeps autoscaling to M40 due to high CPU utilisation. Inserts in this collection are showing up to be consuming highest time/resources

