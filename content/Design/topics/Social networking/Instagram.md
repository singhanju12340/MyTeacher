---
Creation Time: Wednesday, July 10th 2024
Modified Time: Friday, December 6th 2024
---
**Functional Requirements**
_User Management_: The user should be able to create and log in to an account.

_User Activity_ 
1. User should be able to  create, edit and delete a post, reel or status .
2. User should be able to follow and allow to follow other users
3. Allow user to like and comment on other friends post
4. User should be able to view its profile and list of activities he has done
5. User can search friends and post by title
_Friends and Followers_ : 
1. One user can follow multiple users
2. User can accept follow request
3. User can create close friend list and restricted the user to view post.
_User Feed_ : 
1. Provide feed with a list of friends activities user follows.
2. Feed should be rendered based on user interests 


**NFR:** 
1. User Feed should be loaded with minimum latency 200 ms
2. User should be able to post within miliseconds 
3. System should be 99.99% available
4. User post should not be lost and delete unless user deletes it





6. Users should be able to upload/download/view photos.
7. Users can perform searches based on photo/video titles.
8. Users can follow other users.
9. The system should generate and display a user’s News Feed consisting of top photos from all the people the user follows.

**Non-functional Requirements**

1. Our service needs to be highly available.
2. The acceptable latency of the system is 200ms for News Feed generation.
3. Consistency can take a hit (in the interest of availability) if a user doesn’t see a photo for a while; it should be fine.
4. The system should be highly reliable; any uploaded photo or video should never be lost.


The system would be read-heavy, so we will focus on building a system that can retrieve photos quickly.

1. Practically, users can upload as many photos as they like; therefore, efficient management of storage should be a crucial factor in designing this system.
2. Low latency is expected while viewing photos.
3. Data should be 100% reliable. If a user uploads a photo, the system will guarantee that it will never be lost.

**Capacity estimates:**
lets assume: 
500M  user, 1 M active user
2M photo upload daily
average photo size = 200KB

Space for photos: 2M * 200KB = 400M KB/ 1000*1000 = 400GB
Total space required for 10 yr: 400GB * 10 * 365 =  
Network bandwidth for write : 2M daily = 2M*24*60*20 per Sec 
Network bandwidth for read : 


![[Screenshot 2024-07-10 at 1.40.45 PM.png]]

## Data Size Estimation
Let’s estimate how much data will be going into each table and how much total storage we will need for 10 years.

**User:** Assuming each “int” and “dateTime” is four bytes, each row in the User’s table will be of 68 bytes:

UserID (4 bytes) + Name (20 bytes) + Email (32 bytes) + DateOfBirth (4 bytes) + CreationDate (4 bytes) + LastLogin (4 bytes) = 68 bytes

If we have 500 million users, we will need 32GB of total storage.

500 million * 68 ~= 32GB

**Photo:** Each row in Photo’s table will be of 284 bytes:

PhotoID (4 bytes) + UserID (4 bytes) + PhotoPath (256 bytes) + PhotoLatitude (4 bytes) + PhotoLongitude(4 bytes) + UserLatitude (4 bytes) + UserLongitude (4 bytes) + CreationDate (4 bytes) = 284 bytes

If 2M new photos get uploaded every day, we will need 0.5GB of storage for one day:

2M * 284 bytes ~= 0.5GB per day

For 10 years we will need 1.88TB of storage.

**UserFollow:** Each row in the UserFollow table will consist of 8 bytes. If we have 500 million users and on average each user follows 500 users. We would need 1.82TB of storage for the UserFollow table:

500 million users * 500 followers * 8 bytes ~= 1.82TB

Total space required for all tables for 10 years will be 3.7TB:

32GB + 1.88TB + 1.82TB ~= 3.7TB

## Component Design
Photo uploads (or writes) can be slow as they have to go to the disk, whereas reads will be faster, especially if they are being served from cache.

Uploading users can consume all the available connections, as uploading is a slow process. This means that ‘reads’ cannot be served if the system gets busy with all the ‘write’ requests. We should keep in mind that web servers have a connection limit before designing our system. If we assume that a web server can have a maximum of 500 connections at any time, then it can’t have more than 500 concurrent uploads or reads. To handle this bottleneck, we can split reads and writes into separate services. We will have dedicated servers for reads and different servers for writes to ensure that uploads don’t hog the system.

Separating photos’ read and write requests will also allow us to scale and optimize each of these operations independently.

[[Screenshot 2024-07-10 at 1.44.52 PM.png]]

![[Screenshot 2024-07-10 at 2.01.10 PM.png]]

