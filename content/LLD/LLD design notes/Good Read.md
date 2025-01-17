---
Creation Time: Monday, November 11th 2024
Modified Time: Monday, November 11th 2024
---
### **Requirements:**
1. group activity, where all my followers can see my activity
3. book discovery platform, search for a book based on criteira
4. discover a book by some criteria
5. review and rate a book and author
6. user can create his book shelf private or public
7. Set reading goal
8. list of generes
9. listing a book with book images and details
10. activity dashboard to be seen as to followers
11. Show the readers user list, sorted by the order of user priority





### Expectations:
running code - 
rating system
data model
what type of database
high level services
how do we keep book details while listing like media

### Extension:
persona based recommendations 
recommendations based on users past activity
book rating based on fixed questionnaire


#### database design
author book mapping: mapping
book data: in sql 
	
user book shelf 
	what kind of DB, we will be using: sql

user review rating and activity timeline: columnar DB like Cassandra
1. aggregation needed
2. good performing defined query
3. not a transitional data
4. more wright
5. horizontal scaling

**Discover a book:**
1. need fast
2. need some suggestions for users
3. text based search
4. search based on near to name

_DB design:_
User
Book
User Activity
UserActivityParticipants (followers, participants)
	contest/ shelf / channel
Book Review
Author details
Configs for books and 
user reading goal







