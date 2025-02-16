---
Creation Time: Monday, December 2nd 2024
Modified Time: Sunday, February 9th 2025
---
### Functional Requirements
- **Top-N Queries**: Display top N players (e.g., top 10, top 100) on the leaderboard and update it in real-time.
- **Player’s Own Rank**: Allow a player to query their current rank without scanning the entire leaderboard.
- **Rank Update:** The system updates that player’s rank immediately.
- **Nearby Rank Queries:** Provide the ability to retrieve a “slice” of the leaderboard around a specific player (e.g., ranks 45 to 55 if the player is rank 50).
- **History Tracking:** Players can view past game scores and historical leaderboards for previous matches.
### Non-Functional Requirements
- **Real-time updates:** Score changes should reflect immediately in the leaderboard.
- **Low latency**: Leaderboard queries should return the results in milliseconds
- **High Concurrency**: System should support thousands of concurrent players submitting scores and fetching rankings.


**Challenges:**
- Efficiently retrieving Top-N players (e.g., Top 10 or Top 100).
- Allowing players to quickly find their own rank without scanning the entire leaderboard.


### API Design

Update score
	POST /leaderboard/score/update
	Request: 
```Json
{
	"playerId": "player123",
    "scoreDelta": 50
}
```

Response:
```JSON
	{
		"status":"success",
		"currentScore" : 450,
		"currentRank" : 10
	}
```


Get Top N players
	GET / leaderboard/top?n=N
	Response
```Json
[
	{
		"playerId":"player1",
		"currentScore" : 450,
		"currentRank" : 10
	}
]
```

Get player rank
GET /leaderboard/rank/{playerID}
```Json
{
	"rank" : 1,
	"playerId" : "playerId",
	"currentScore" : 23
}
```

Get Near by rank
GET /leaderboard/nearBy/rank?range=2
```JSON
{
	"playerId": "player1",
	"currentRank" : 6,
	"currentScore" : 234,
	"players" :[
	{
		"playerId": "player4",
		"currentRank" : 4,
		"currentScore" : 234
	},
	{
		"playerId": "player3",
		"currentRank" : 5,
		"currentScore" : 234
	},
	{
		"playerId": "player5",
		"currentRank" : 7,
		"currentScore" : 234
	},
	{
		"playerId": "player5",
		"currentRank" : 8,
		"currentScore" : 234
	}
	]
}
```