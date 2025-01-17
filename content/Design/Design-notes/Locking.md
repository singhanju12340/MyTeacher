---
Creation Time: Monday, December 2nd 2024
Modified Time: Monday, December 2nd 2024
---

## Pessimistic vs Optimistic Locking
_Pessimistic locking assumes conflicts will occur and locks the data before any changes are made. It prevents other users from accessing and updating the data until the lock is released._

_Optimistic locking assumes conflicts are rare. It allows multiple users to access data simultaneously and checks for conflicts when changes are committed. If a conflict is detected, the operation is rolled back.



Some best practices to consider:

1. Hold locks for the minimum possible time to reduce contention.
    
2. Apply locks at the most granular level such as rows rather than tables.
    
3. Implement retry logic for transactions that fail due to conflicts.
    
4. Pessimistic locking is better for data integrity but can impact performance.
    
5. Optimistic locking is better for efficiency and performance._

