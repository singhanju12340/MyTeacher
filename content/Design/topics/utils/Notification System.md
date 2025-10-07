
**FR:**
1. **Notification Creation and Management:** The system should allow authorized users to create, edit, and delete notifications. This includes specifying the content, type (e.g., text, image, video), and urgency of the notificatio
2. Send differant type of message to users
3. Notifications must be able to be sent to specific users, groups, or segments based on criteria like user roles, demographics, or activity.
4. - The user can provide their preference on how they want notifications to be sent.
5. - We should avoid processing the same notification multiple times by using an idempotency key from the client. If the idempotency key is not present, all notification requests will be treated as new.
**Out of Scope:**
- Bulk notifications: Sending the same notification to a large group of users is a bulk service, not a real-time notification service.

NFR:
1. **Scalability**: The system must be able to handle a large and increasing volume of notifications and users without performance degradation
2. **Real Time:** The system must deliver notifications to users' devices (web, mobile, desktop) in real-time or near real-time.
3. **Security**: The system must ensure that user data and notification content are secure


__Traffic Estimations:
- Total number of notifications: 100 million
- TPS: 10*100^6 /10^5 = 1000 message/sec
- Assume on peak time 10 times = 10000 message/sec

API Design:

Database Design:

Basic Design completing FR:

Challenges and solution to fulfill NFR: 



Maintenance:
Monitoring/ Analysis/ Securities/ Ratelimiting / Circuit breaking 

