requirements:
1. user should be able to book
2. list all movie for upcoming month
3. view booked history by date filter
4. list all movie in theater for decided location diameter.
5. keep movie detail with actor, actress, rating
6. keep movie language details
7. book other event
8. cinema can be in different halls
9. halls can have list of chairs
10. chairs can have categories
11. each chair can have different price range
12. seat should be identifies as booked or vacent
13. 


**Actor:**
	Admin
	User
	guest

**Usecases:**
Admin: 
	crud  movie
	crud show
	crud halls
guest:
	search movie
customer: 
	search movie
	book movie
	cancel booking
	
System:
	generate notification

**Activity diagram for booking a movie:**




**Class Diagram:**
Entities:
address
User: 
city:
cinemaHall:
show:
cinemaHallSeat:
Ticket:
Booking
payment
notification
![[ClassDiagram.png]]

