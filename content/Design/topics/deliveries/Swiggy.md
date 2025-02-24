---
Creation Time: Thursday, July 25th 2024
Modified Time: Sunday, February 23rd 2025
---
### Requirement  (15 min)
User should be able to search food menu
User should be able to filter menu based on cuisine, food category
User should be able to add/remove items into the cart
User should be able to make payment via third party and place order
User should be able to see estimated delivery time for each restaurant while browsing the menu
User should be able to track food delivery status : restaurant accepted/ food in preparation / food prepared / pickedup / delivered
User should be able to track location of the delivery partner
User should be able to see out of stock item with out of stock tag

Out of scope
User should get notified about the status of food, and get notification when delivery  partner is in proximity.
Restaurant should register itself on swiggy
restaurant should upload menu

### NFR
Places order should not get lost
System should be consistent, once user place an order it should get delivered and tracked
System should be available but consistency is more important.
System should be made distributed and data should be replicated to avoid data loss
system is read heavy

### Capacity estimates
DAU: 100K
order per day by each user: 1 order
Order TPS: 100K/86400 ~ 1TPS
Search menu average 5 times more than order: 5TPS




### API design
GET /v1/restaurants/search? lat = 24233.3423 & long = 232113.231 & radius=3
User token should be passed in api header
Response:
```
	"status":"success",
	"body":[
		{
			"name" : "khana khajana",
			"id" : "21342",
			"location" : {"lat" : 42342341123, "long" : 23423423423},
			"cuisine" : [],
			"openTime" : 9:AM,
			"closingTime" : "10:PM",
			"img" : "https://nsdkjnak/kjsndkjsn.jpg"
		}
	]
```

GET /v1/menu/{restaurantId}/search
Response:
```
	status: "success",
	"body" : [
		{
			"id":"32323",
			"name" : "roti",
			"price" : 40,
			"img" : "https://kbsankjfn/roti.jpg",
			"foodType" : "north-indian",
			"veg" : true
		},
		{
			"id":"32323",
			"name" : "sabji",
			"price" : 40,
			"img" : "https://kbsankjfn/roti.jpg",
			"veg" : true
		},
		{
			"id":"32323",
			"name" : "chicken",
			"price" : 400,
			"img" : "https://kbsankjfn/chicken.jpg",
			"veg" : false
		}
	]
```

POST /v1/cart/add?userID=123123
Request:
```
	
		{
			"itemId" : 21321,
			"quantity" : 2,
			"restaurantId" : "asdasa" 
		}
	
```

GET /v1/cart/search?userID=123123
Response:
```
	
		status: "success",
		body:{
			"items": [
				{
					"id":"32323",
					"name" : "chicken",
					"price" : 400,
					"img" : "https://kbsankjfn/chicken.jpg",
					"veg" : false,
					"quantity" : 2
				}
			],
			"restautantId" : "sadasd",
			"totalPrice": 23423,
			"priceAfterDiscount" : 23123
		}
```

POST /v1/order/placeOrder?userID=123123
Response : 
```
{
			"status": success,
			"orderEstimatedTime" : 30 min
			"orderStatus" : "resturant accepted",
			"orderId" : ksndkas,
			"resturantId": "33232",
			"restaurantContactNo" :"324235345"
}
```

GET /v1/order/search?userID=123123 & orderID=ksndkas
Response:
```
{
			"status": success,
			"estimatedDeliveryTime" : 15 min
			"orderStatus" : "food prepared",
			"orderId" : ksndkas,
			"resturantId": "33232",
			"restaurantContactNo" :"324235345"
}
```


### DB schema
```
User
	id,
	savedLocation: [
		{
			
		}
	],
	email
	phoneNo

Restaurants
	id
	name
	location (index)
	cuisine:[], (index)
	vegOnly
	img
	restaurantMenu

Order
	userId (index)
	orderId
	totalAmount
	restaurantId
	menuList[]
	orderDate (index)
	orderTime
	deliveryTime
	txnId
	
```

### HLD



### DB Choices
No sql to keep menu and restaurant may be elastic search because if provides better search, filter and autosuggestions functionality
USer details in mysql or nop sql
User order in nosql such as postgres
keep deliver partner latest lat and long into redis for quick access
DP can sent continous location tracking using websocket connection to location service
Top hits restaurants menu can be cached to avoid loading and provides fast searches
restaurants data should be kept in postgress db as it provides GEO spatial queries which will be used to find near by restaurants for given user location.


