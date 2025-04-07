---
Creation Time: Monday, February 24th 2025
Modified Time: Monday, February 24th 2025
---
### Requirements:
1. Show top k most viewed video within time frame 1 hours, day, 1 month, from given streams of views.
2. return only max 1000 videos
3. timeframe should be sliding window

Out of scope:
No  arbitrary starting point


### NFR:
4. after how many min of video view, it should be calculated in top view. less than 1 min
5. 1-100 ms read latency
6. massive view
7. massive videos
8. Too many host should not be used to calculate top  views
9. No approximates
10. response should be paginated

### Capacity Estimates:
70B views per days

>> Views count:  70 * 10^9 / 86400 = 70 * 10^4 = 700K /sec  ~ 1M views/ sec

youtube gets 
>>  1M video / day ~ 3.6B videos in 10 yr ~ 3.6 GB *  (data needed for 1 video metadata) ~ 3.6GB * 8 byte ~ 30GB



### API designs
GET /v1/top/views/count=1000
Request:
```
{
	"duration" : Min/hour/Day/All
	"maxCount" : 1000
}
```
User auth token in header
response:
```
{
	"status" : success,
	"data" :
	[
		{
			"videoUrl" : "hjsabdajknd",
			"views" : 20
		}
	],
	"page" : {
		"start" : "",
		"end" : ""
	}
	
}
```

Save View count
	inout: videoId
	