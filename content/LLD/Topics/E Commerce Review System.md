---
Creation Time: Monday, July 1st 2024
Modified Time: Tuesday, September 24th 2024
---

**Design a review system for retailer**

User should be able to add review text for a product  
User should be able to provide overall rating for the product  
Users should be able to see the summary of ratings  
Users should be able to review different features of the product and rate them separately  
Users should be able to add images of the product which is being reviewed Users should be able to filter reviews based on the below

1. My reviews
2. Get By product
3. Get reviews by feature
4. Top rated reviews
5. Reviews by date Asc and Desc
6. Get certified reviews
7. Get reviews having images

Model:

```plantuml
@startuml
class User{
 id,
 name,
 username,
 password
}

class Order{
userId,
itemsOrderMap
purchaseDate
}

class userItemOrder{
	orderId
	itemId,
	reviewList
	reviewStatus
	reviewCertified
	reviewDate
}

enum reviewType{
	size, color, quality, packaging
}

class review{
	reviewType
	description
	imagelink
	rating
}

class Item{
	id,
	name,
	description,
	images,
}

enum ReviewState {
    MODERATION_SUCCESS,
    MODERATION_PENDING,
    MODERATION_FAILED
}

enum ReviewSentiment {
    POSITIVE,
    CRITICAL,
    DETAILED,
    GENERAL
}

@enduml
```

UserService{
	addUser();
	blackListUser();
	
}

OrderService{
	
}

ReviewService{
	certifyReview(reviewId);
	List<Review> getCertifiedReview()
	fetchReview(Filter filter)
	fetchAllReviewByUser(userId)
	
}