---
Creation Time: Tuesday, January 21st 2025
Modified Time: Wednesday, January 22nd 2025
---
### Functional requirement:
- The system should allow parents and school administrators to track buses in real-time.
- Notifications should be sent when the bus is near a specific location or running late.

### NFR
System should be scalable, to handles ,multiple buses and multiples schools across country
Data should be secured, as its a minor related application data
Only the subscribed/logged in parents should be able to track only bus which carries their kids



#### What are the main components of your system?

> The main components would be:
> 
> 1. **Bus Device**: A GPS device or mobile app installed on the bus to send location updates.
> 2. **Backend Service**: A central service to ingest and process location data.
> 3. **Real-Time Tracking**: A service to broadcast location updates to parents and school administrators.
> 4. **Notification Service**: Sends alerts for events like proximity to a stop or delays.
> 5. **Database**: Stores historical data, user profiles, and route information.
> 6. **Frontend**: A web/mobile application for parents and admins to view the bus's location.


#### How would you handle real-time location updates efficiently?

> For real-time updates:
> 
> - **Data Ingestion**: Use a message queue like Kafka or RabbitMQ to handle GPS updates from multiple buses.
> - **Data Processing**: A stream processing system like Apache Flink or Spark Streaming would process the updates and compute proximity to stops or delays.
> - **Broadcast Updates**: Use WebSockets or a real-time messaging protocol like MQTT to push updates to subscribed clients (parents and admins). Use Google Maps API/Mapbox for MAP rendering
> - **Optimization**: To reduce load, batch location updates or send them at regular intervals (e.g., every 10 seconds).


#### How would you design the notification system?

> Notifications can be event-driven:
> 
> - **Proximity Alerts**: When the bus is within a defined radius of a stop, trigger a notification.
> - **Delay Notifications**: Calculate ETA using historical traffic patterns and live data; notify users of delays.
> - **Implementation**:
>     - Use an event-based architecture with Kafka to publish proximity/delay events.
>     - A Notification Service processes these events and sends messages via SMS, push notifications, or email using providers like Twilio or Firebase Cloud Messaging.


#### How would you ensure scalability as the number of schools and buses grows?


> To ensure scalability:
> 
> - **Microservices Architecture**: Separate services for location tracking, notifications, and user management to scale independently.
> - **Load Balancing**: Use a load balancer like AWS ALB or Nginx for backend services.
> - **Database Sharding**: Partition the database by school ID or region to distribute the load.
> - **Caching**: Use a distributed cache like Redis to store frequently accessed data like current bus locations.
> - **Horizontal Scaling**: Deploy services on a Kubernetes cluster for dynamic scaling based on load.


#### How would you ensure data security and privacy?

> Security is critical since this system involves minors. Key measures:
> 
> - **Authentication and Authorization**: Use OAuth2 for secure access. Ensure that only authorized users can access bus location data.
> - **Data Encryption**: Encrypt sensitive data in transit (using HTTPS) and at rest (using database encryption).
> - **Access Control**: Implement role-based access control (e.g., parents can only view their child’s bus).
> - **Audit Logging**: Maintain logs for all user actions to detect unauthorized access.


#### How would you handle situations where GPS updates are delayed or lost?

> - **Fallback Mechanism**: Use the last known location and calculate an estimated trajectory.
> - **Data Validation**: Discard stale or duplicate GPS updates based on timestamps.
> - **Redundancy**: Use backup GPS devices or mobile networks to minimize disruptions.



### **Hybrid Approaches**

- Combine multiple methods based on the use case:
    - Use **push notifications** for critical updates when the app is offline and **WebSockets** when the app is active. using **Firebase Cloud Messaging (FCM)** or **Apple Push Notification Service (APNS)** platforms
    - Use **long polling** as a fallback mechanism if WebSockets fail.

![[Pasted image 20250202131717.png]]