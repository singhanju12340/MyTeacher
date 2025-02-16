---
Creation Time: Wednesday, February 5th 2025
Modified Time: Wednesday, February 5th 2025
---
### Offset Pagination 
The offset is set to 0 to load the most recent comments, and it increases by the number of comments fetched each time the user scrolls to load more

###### Challenges
First, offset pagination is inefficient as the volume of comments grows. The database must count through all rows preceding the offset for each query, leading to slower response times with larger comment volumes. Most importantly, offset pagination is not stable. If a comment is added or deleted while the user is scrolling, the offset will be incorrect and the user will see duplicate or missing comments.
Offset pagination  Uses `LIMIT OFFSET`, which scans unnecessary rows, Cursor pagination uses where clause in DB and avoid unnessecery row scans.
Offset pagination should be used only to fetch historical data, where there is no further addition or updation in the data. Also for small scale of data

```
SELECT * FROM comments ORDER BY created_at ASC LIMIT 20 OFFSET 40;
```


### Cursor Pagination
It uses where clause with index query. (`WHERE id > last_seen_id`) instead of `LIMIT OFFSET`, avoiding scanning large data sets.
Even if you have millions of comments, **cursor pagination remains fast**.
It provides cursor to specify the starting point for fetching a set of results. and cursor is updated each time the user scrolls to load more.

Example: 
```
SELECT * FROM comments WHERE created_at > '2024-02-05 12:00:00' ORDER BY created_at ASC LIMIT 20;
```