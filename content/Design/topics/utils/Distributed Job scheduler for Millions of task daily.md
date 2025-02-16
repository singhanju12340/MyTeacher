---
Creation Time: Monday, February 3rd 2025
Modified Time: Tuesday, February 4th 2025
---
`Example use case
 send password reset email 
  marketing email collection
  payment remainder

### Functional:

- User actions: **Create and delete jobs. Retrieve a list of jobs created by the user. View the execution history for any given job.**
- Job execution style: **The system should support both one-time execution, recurring style**
- ==Number of average estimated daily tasks executions:== ==**50 million tasks each day**==
- Task fail strategy: **The system should have the capability to support a _retry_ feature**
- Our system is configured to perform task executions **every minute**

MY Functional requirement
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
14. The system should be of low latency. Should be able to run thousands of tasks without daily.
15. Schedule data should be persisted, and should not loss in case of system failure.
16. The System should be scalable, it should support more number of task scheduled with time, such as 100K.
17. The System should be secured enough, to protect against unauthorised access.
18. The System should provide monitoring capabilities to track health and performance of the system.
19. System should have auditing and logging to track task scheduled and events published in the system

### Capacity estimates:
jobs to be executed = 50 M / Day
If we would average system load **per minute** based on this formula (daily tasks)/(24*60) => 50 000 000/24*60 ≈ 34722 tasks/minute


## HLD
<img>
![[Pasted image 20250203232755.png]]

### Tables Schema

`Job
Jobs list will be fetched by given userid, so we will keep userId as a partition key to enhance data locality. use jobid as sort key inside the partition.

| id   | name | description | scheduleType | User ID(partition key) | Max retry count | is_recurring | created at | end date |
| ---- | ---- | ----------- | ------------ | ---------------------- | --------------- | ------------ | ---------- | -------- |
| job1 | job1 |             | PT3H         | User 1                 | 3               | yes          |            |          |
we need another table to store the next execution time for each job. By doing this it will be easy for us to query all scheduled tasks for the current minute(as we execute our tasks in each minute)
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



## 2. Potential Interview Follow-Up Questions

Below are **common topics** an interviewer might explore to assess your depth in distributed system design and operational concerns:


` **“How do you handle super long-running jobs vs. short batch jobs?”**`
_Separate Queues or Pools for short jobs and long running jobs
**Pros**:
- Simple conceptual separation.
- Operators can tune each queue (time limits, priorities) differently.
**Cons**:
- Requires users to choose the correct queue.
- Might lead to suboptimal overall usage if one queue is idle while the other is heavily loaded.
_Priority-Based Scheduling with Preemption
- **Idea**: Assign higher **priority** to short or latency-critical tasks, allowing them to preempt (pause or evict) lower-priority tasks if no other resources are available.
- **Long-running jobs** may get lower priority or remain in “background mode,” using only free resources.
- If the system supports **checkpointing** or **application-level state saving**, the long job can resume later without losing all progress.
**Pros**:
- Ensures short tasks get resources fast.
- Maximizes cluster usage by letting big jobs run in background when resources are free.
**Cons**:
- Preemption overhead: if the long job can’t checkpoint, losing partial progress is expensive.
- Must carefully define priority classes and preemption rules to avoid thrashing.
_Reservation Systems_
For extremely long or large jobs, the user can **reserve** resources in advance. Can reduce overall cluster utilization if resources are reserved far in advance and sit idle awaiting the job.
 **Dedicated Partitions**: If certain nodes or clusters specialize in long HPC (High-Performance Computing) workloads, you can isolate them from short job traffic.
  **Time Limits**: Impose maximum job durations. If a job exceeds it, either fail or require special approval.
Some HPC schedulers use “backfill” for short jobs in the idle time while waiting for a large job’s reservation window to start.


`    - **“How can new types of jobs (e.g., new container runtime or custom executors) be integrated?”**`
Defining the “Job Type” or “Runtime” in the job
## Example Flow
**1.** The operator registers a new job type: “gpu-pyTorch” (requiring a special container runtime).  
**2.** The executor master sees `runtimeType = "gpu-pyTorch"` in the job spec.  
**3.** The executor master  picks a node labeled “gpu-node” that has the `GpuPyTorchExecutorPlugin`.  
**4.** The node agent calls `GpuPyTorchExecutorPlugin.launchJob(jobSpec)`, which:

	- Pulls a specialized container image or configures GPU drivers.
	- Launches the container or process.
	- Streams logs/status back to the agent. 
The agent updates the executor with the job’s state.

`How will you handle ordering of dependent jobs execution`
job.dependsOn = [...]
**Maintain** Job States:
- `PENDING`, `RUNNING`, `SUCCESS`, `FAILED`, etc.
- Possibly `BLOCKED` or `WAITING_ON_DEPENDENCIES`.
 **Transition Logic**:
    - Each time a job finishes, check if that unblocks dependent jobs.
    - If so, move them to `PENDING` or **submit** them automatically.