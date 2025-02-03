---
Creation Time: Sunday, February 2nd 2025
Modified Time: Monday, February 3rd 2025
---
#### Functional Requirements:
1. Operator should be able to schedule batch for students
2. User can be present in multiple batch
3. Batch should not be conflicting, with other batch if user is common in 2 batch, and should show conflict before booking
4. Student should be able to fetch list of batch for given month or week range only for his enrolled batch
5. User should be able to see all scheduled batch for given date range
6. Same batch should not be scheduled for the same time slot twice
7. Schedule  batch only for future dates
8. All student should receive notification for new scheduled batch
9. All enrolled student should get notification before 30 min of the class
10. Operator can see available free slot for given batch and date.
11. batch can be recurring 
12. batch can be scheduled for any time duration


#### NFR:
1. Data should not get lost
2. Read latency should be minimal
3. Its a read heavy system
4. concurrency should be handled

#### Capacity estimates:
Read : 10K / Sec
Write: 10/Sec
Batches count: 20K all across country


### Tables schema
`User 

| id  | name    | age | Role     |
| --- | ------- | --- | -------- |
| U1  | U1 name | 18  | Student  |
| U2  |         |     |          |
| U10 |         |     | Operator |

`Batch

| Id  | name    | description | tag       |
| --- | ------- | ----------- | --------- |
| B1  | B1-name | des         | Math      |
| B2  |         |             | Physics   |
| B3  |         |             | Chemistry |
|     |         |             |           |

`User Batch

| Id  | user Id | batch Id | active | created at |
| --- | ------- | -------- | ------ | ---------- |
| 1   | U1      | B1       |        |            |
|     | U2      | B1       |        |            |
|     | U3      | B1       |        |            |
|     | U4      | B1       |        |            |
|     | U1      | B2       |        |            |
|     | U5      | B2       |        |            |
|     | U6      | B3       |        |            |

`Batch Schedule

| Id  | start time          | end time         | batch Id | isRecurring | recurring pattern |
| --- | ------------------- | ---------------- | -------- | ----------- | ----------------- |
| BS1 | 14/02/2025 10:00 AM | 14/02/2025 11 AM | B1       |             | {}                |

`User Schedule`

| id  | user Id | start time | end time | batch schedule id |
| --- | ------- | ---------- | -------- | ----------------- |
|     | U1      | 10:00      | 11:00 AM | BS1               |
|     |         |            |          |                   |

`+ve scenario

B1 has to be scheduled for 14/02/2025 from 10AM  to 11 AM


`-ve scenario`
B2 has to be scheduled for 14/02/2025 from 10AM  to 11 AM after B1 is scheduled








