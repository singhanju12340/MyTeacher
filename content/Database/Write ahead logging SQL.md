---
Creation Time :
Modified Time :
Creation Time: Monday, July 29th 2024
Modified Time: Monday, November 18th 2024
---

Write to log file before actually writing it in table storage and managing index changes.
Flush from log to tables once a while, if crash happens log actions can be replayed
Can get point in time snapshot using this write ahead logging
Can restore db upto a specific time slot.
Data integrity: every individual record is CRC-32(Checksum) 
![[Screenshot 2024-07-29 at 5.24.19 PM.png]]Log File lines start with byte offset, we can directly go to particular offset and replay from that point.

| byte offset | CRC | Query statement             |
| ----------- | --- | --------------------------- |
| 0000        | CRC | Update name from user where |
| 0002        | CRC | Insert age in user          |

