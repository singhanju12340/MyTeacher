![[Screenshot 2024-06-17 at 8.05.27 PM.png]]

Features to be implemented:
1. keep all types of format: ODI, Test match
2. keep track of teams and their matches
3. show ball by ball commendatory
4. Tournament should be announced
5. team playing should announce teams(team eleven)
6. Keep stats of tournament, teams, matches
7. provide stats details based on the filters

**Use case diagram:**
Actor: 
	commentator
	admin
	User
		

Activity:
Admin:
		add/modify player
		add ball
		add team
		add tournament
		add over 
		add empire/referee
	
Commentator:
	add/modify comments
	view comments
User:


**Class diagram:**
	Team
	Player
	Tournament
	match (ODI, TEST, T20)
	stats extended by player, team, tournament
	inniing
	ball
	commnetator

	
![[ClassDiagram.jpg]]