---
Creation Time: Monday, July 29th 2024
Modified Time: Wednesday, January 8th 2025
---

A **Flash Sale System** allows a large number of users to purchase limited-stock items in a short period, often leading to high traffic and race conditions. The system must handle concurrency, scalability, and fairness effectively.

## Functional:
- **Product Management**:
    
    - Admins can configure products for the flash sale with details like stock, price, and sale start time.
- **User Participation**:
    
    - Users can view products and attempt to purchase during the sale.
- **Purchase Handling**:
    
    - Ensure fair order allocation based on "first-come-first-served" principles.
    - Deduct stock atomically for successful purchases.
    - 1 inventory should be sold to only 1 user.
- **Notifications**:
    
    - Notify users about successful or failed purchases.
- **Sale Summary**:
    
    - Generate a summary of products sold and remaining stock.


## NFR: 
_High availability_: System should be available
_High concurrency_: millions of request concurrently
_Consistency_: Ensure no stock is oversold. Use locking mechanisms (e.g., Redis locks) to prevent overselling of stock.
_Latency_: quick responsiveness
_Fairless_ : Queue purchase requests to ensure fair processing and avoid overloading the system.



#### Frontend:
1. Keep less web elements on webpage to reduce loading time
2. Use pagination to load fast inventory and fetch the next slot as the user scrolls 
3. use CDN to cache static content
4. Use Captcha integration before placing an order to prevent robots 

#### Risk Control at NGINX and API Gateway:

1. Have a system check in place to deduct and prevent DDos Attack
2. A rate limit should be placed on the number of request from single IP.
3. Suspicious IPs should be banned to access flash sale
#### Over Sale prevention
1. Lock inventory once user place an order
2. decrement inventory count once payment for an order is successfull.
3. Use Redis to add distributed locking

#### Use of Cache
1. Keep flash sale inventory in Cache, and update DB in batch separately

#### Backend Services
1. Availability: 
	 - Use isolated instances for flash sale (If the flash sale instances go down, other normal instances will not be affected).
	 - Use isolated cache for flash sale (If the flash sale cache goes down, other normal cache will not be affected).


### **HLD Components**

1. **API Gateway** NGINX:
    
    - Acts as the entry point for all user requests.
    - Ensures authentication and rate-limiting.
    - Add Circuit Break Hystrix to stop accepting request after certain number of failures to stop catastrophic.
1. **Load Balancer** NGINX: 
    
    - Distributes incoming traffic across multiple app servers.
3. **Application Servers**:
    
    - Handle user requests (order placement, stock checks).
    - Interact with the product catalog and order service.
4. **Cache Layer (Redis)**:
    
    - Stores product inventory for fast access.
    - Implements distributed locks for atomic stock deduction.
5. **Database**:
    
    - Stores persistent product, order, and user data.
    - Maintains transaction logs for audit and recovery.
6. **Notification Service**:
    
    - Sends real-time notifications for order outcomes.
7. **Message Queue**:
    
    - Queues order requests to ensure sequential processing and avoid overload.
8. **Analytics System**:
    
    - Tracks user behaviour and generates sales reports.



[[Drawing 2025-01-08 12.05.12.excalidraw]]
