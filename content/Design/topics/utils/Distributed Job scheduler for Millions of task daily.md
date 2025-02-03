---
Creation Time: Monday, February 3rd 2025
Modified Time: Monday, February 3rd 2025
---
### Functional:

1. User should be able to schedule task based on task name, schedule time and recurrence via some web application
2. The End date should always be greater than start date for the recurring scheduling.
3. The User should be able to fetch, edit and delete event based on eventId.
4. A Unique event id should be generated for each event.
5. The Recurring schedule can be daily, monthly, weekly, or some standard pattern like first Monday of every month.
6. The user should be able to fetch all of its scheduled tasks.
7. The User should be able to fetch task run history for given taskId with success/failure status.
8. Recurring tasks should have an end date.
9. Provide start time for recurring tasks as an optional parameter.
10. schedule time should be greater than the current time
11. if multiple Tasks are scheduled for the same time, start tasks based on priority. add priority while scheduling tasks.
12. User should have limits on create schedule, to avoid DDoS attack.

### NFR
1. The system should be of low latency. Should be able to run thousands of tasks without daily.
2. Schedule data should be persisted, and should not loss in case of system failure.
3. The System should be scalable, it should support more number of task scheduled with time, such as 100K.
4. The System should be secured enough, to protect against unauthorised access.
5. The System should provide monitoring capabilities to track health and performance of the system.
6. System should have auditing and logging to track task scheduled and events published in the system

### Capacity estimates:
jobs to be executed = 50 M / Day


## HLD

![[Pasted image 20250203232755.png]]

### Tables Schema

`Job

| id   | name | description | scheduleType | User ID | Max retry count | is_recurring | created at | end date |
| ---- | ---- | ----------- | ------------ | ------- | --------------- | ------------ | ---------- | -------- |
| job1 | job1 |             | PT3H         | User 1  | 3               | yes          |            |          |

`task schedule`

| job id(partition key) | next_execution_time |
| --------------------- | ------------------- |
| job1                  | 2024-01-21T00:01    |
| job2                  | 2024-01-21T00:03    |
| job3                  | 2024-01-21T00:05    |
| jobn                  | 2024-01-21T00:01    |
_query run on every minute_
select * from task_schedule where next_execution_time = '21/01/2024:00:01'

_Finding based on next execution time where job_id is partition is will lead the query to run in multiple partition.

| job id(sort key) | next_execution_time(partition key) |
| ---------------- | ---------------------------------- |
| job1             | 2024-01-21T00:01                   |
| job2             | 2024-01-21T00:03                   |
| job3             | 2024-01-21T00:05                   |
| jobn             | 2024-01-21T00:01                   |
All row will  with same minute will be stored in same db partition. fetching task to execute will be faster with `next_execution_key` as partition key. 
In case of race condition, if two different instances attempt to poll and execute the same task simultaneously, the operation _may be executed twice_. This scenario poses a serious problem, especially for critical tasks such as sending notifications to customers, generating invoices or performing payment operations. 

| job id(sort key) | next_execution_time | segment |
| ---------------- | ------------------- | ------- |
| job1             | 2024-01-21T00:01    | 1       |
| job2             | 2024-01-21T00:03    | 2       |
| job3             | 2024-01-21T00:05    |         |
| jobn             | 2024-01-21T00:01    | n       |
`Composite parition key` :(next_execution_time, segment no)
worker will be assigned random segment number, worker will pick task matching with segment number to distribute worker task. to avoid race condition.
worker will run query based on segment no assigned to worker node
_select * from task_schedule where next_execution_time = 1705788060 and segment in (1,2)_

`task schedule history`

| job Id(partition key) | execution time ( sort key) | status    | retry count | last_update_time |
| --------------------- | -------------------------- | --------- | ----------- | ---------------- |
| job1                  | 2024-01-21T00:01           | COMPLETED | 1           |                  |
| jobn                  | 2024-01-21T00:01           | FAILED    | 3           |                  |
|                       |                            |           |             |                  |


