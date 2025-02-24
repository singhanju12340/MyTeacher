---
Creation Time: Monday, July 29th 2024
Modified Time: Saturday, February 22nd 2025
---
### Requirements:
User should be able to login securely.
User should be able to search upcoming flights from given date range and destination
User should be able to watch booked history
User should be able to book flight, and make a payment on the basis of first come first server.
Flight can be connected 0 to 4 hop.
User can book round trip or multi city flight as well
Show users recent searches.
Show flights in sort order based on: price, connecting, time duration, departure time, arrival time.
Show only non connecting flight.

_Not in scope_
Build Notification system for alerts
Build analytical system
Subscription system

### NFR:
Book flight only if payment is successful.
Fast flight searches. search api response within 500 ms.
User from globe can book flight.
Highly available search system.(if admin added flight if users see that after some latency its ok)
Highly consistent booking system.
Booking system needs to handle concurrent booking


### Capacity estimates:
Daily active search: 1M
Each flight has around seat count:  150 seat max
Flight counts daily: 1k
Daily booking: 150K = 1K * 150  ~ 150K / 86400

### API:

`GET: /v1/flight
Request body:
```JSON
{
	"date": "232",
	"source" : "delhi",
	"destination" : "bangalore",
	"connecting" : true
}
```

Response:
```JSON
{
	"status" : "success",
	"body" : [
		{
			"id": "hbksajdnkas",
			"startTime" : "9:00AM",
			"reachTime" : "11:00 AM",
			"price" : "2342342"
			"airline" " airindia",
			"flightDetails":[
				{
					"source" : "delhi",
					"destination" : "bangalore",
					"startTime" : "9:00AM",
					"reachTime" : "11:00 AM"
				}
			]
		},
		{
			"id": "as34fdf",
			"startTime" : "9:00AM",
			"reachTime" : "3:00 PM",
			"price" : "2342342"
			"flightDetails":[
				{
					"source" : "delhi",
					"destination" : "hydrabad",
					"startTime" : "9:00AM",
					"reachTime" : "10:00 AM"
				},
				{
					"source" : "hydrabad",
					"destination" : "bangalore",
					"startTime" : "01:00PM",
					"reachTime" : "3:00 3M"
				}
			]
		}
	]
}
```


`POST: /v1/flight
Header: user auth token
Request body:
```JSON
{
	"id":"hbksajdnkas",
	"customerId":"hjkadsjka"
}
```

Response:
```JSON
{
	"status" : "success",
	"body" : {
			"id": "hbksajdnkas",
			"startTime" : "9:00AM",
			"reachTime" : "11:00 AM",
			"price" : "2342342",
			"airline" " airindia",
			"flightDetails":[
				{
					"source" : "delhi",
					"destination" : "bangalore",
					"startTime" : "9:00AM",
					"reachTime" : "11:00 AM"
				}
			]
		}
}
```

##### Admin APIs

GET /v1/flight?id=sadkasmd
Response:
```JSON
{
			"id": "hbksajdnkas",
			"startTime" : "9:00AM",
			"reachTime" : "11:00 AM",
			"price" : "2342342"
			"flightDetails":[
				{
					"source" : "delhi",
					"destination" : "bangalore",
					"startTime" : "9:00AM",
					"reachTime" : "11:00 AM"
				}
			]
		}
}
```



### DB Schema

_Carrieer details
```
id
name
airlines
type
total seats
total exits
seatDetails{
	normal: [1-100],
	"exitSeat": [22-45]
}
```

_Schedule flight details details
```
id
carrier_id
source_destination  (index)
departureDate(partition key)
departureTime 
endTime
isBookingOpen(filter)
allowedBaggage:
{
	"cabin":"",
	"checkin":""
}

index on source+destination as search are based on this

```
_Flight booking seat details
```
scheduleFilghtId
departureDate
seatDetails: {
	"booked": [],
	"available" : []
}
```
Flight route details history
```
id
scheduleFilghtId,
routeTiming:[
	{
		"scheduledepartureTime": "",
		"scheduleEndTime": "",
		"actualStartTime": "",
		"actualEndTime": "",
		"source": "",
		"destination": ""
	}
]
```
booking details
```
flightId
customerId,
txnId,
amount,
seatId,
bookingDate,
flightRouteDetailHistoryId
```

### Storage estimates
`Flight details
assume 100 KB data size
100KB * 1K * 365 * 5 ~ 2000 * 100MB ~ 200GB 

`Seat booking details
assume 100KB
100KB * 150 seats * 365 * 5  ~ 30GB
### HLD
![[Pasted image 20250222224442.png]]

### DB choices

### possibles Challenges  and tradeoff


[[Two phase commit]]
