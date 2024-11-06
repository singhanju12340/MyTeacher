---
Creation Time: Tuesday, November 5th 2024
Modified Time: Tuesday, November 5th 2024
---
Total Events: 10 lakhs

| .Time | Events lag | time diff | event processed |     |
| ----- | ---------- | --------- | --------------- | --- |
| 1.35  | 280,056    |           |                 |     |
| 1.48  | 240,557    | 13 min    | 40K             |     |
| 1.59  | 208,257    | 9 min     | 32K             |     |
| 2.06  | 188,744    | 7 min     | 12K             |     |
| 2.09  | 180,044    | 3 min     | 8K              |     |
| 2.18  | 162,245    | 9 min     | 18K             |     |
| 2.36  | 107,252    | 18 min    | 55K             |     |
| 2.51  | 56,953     | 15 min    | 51K             |     |
| 2.52  | 49,553     | 1 min     | 7K              |     |
| 2.54  | 42,553     | 2 min     | 7K              |     |
| 2.55  | 40,553     | 1 min     | 2K              |     |
| 2.59  | 31,054     | 4 min     | 9K              |     |
| 3.15  | done       | 15 min    | 31K             |     |

**Current time taken matrix**

|                                    |            |            |            |            |            |            |            |
| ---------------------------------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
| Task name                          | time in ms | time in ms | time in ms | time in ms | time in ms | time in ms | time in ms |
| VALIDATE                           | 0          | 1          |            | 1          |            |            |            |
| FETCH_DC_DEALER_DETAILS            | 0          |            |            |            |            |            |            |
| IMS_DATA_DOWNLOAD_FROM_STORAGE     | 21         | 18         | 6          | 27         |            |            |            |
| TRANSFORM_TO_SEARCH_ES_PAYLOAD     | 0          |            |            |            |            |            |            |
| TRANSFORM_TO_IMS_ES_PAYLOAD        | 1          | 1          |            |            |            |            |            |
| ADD_CS_DEALER_DETAILS_IN_SEARCH_ES | 0          |            |            |            |            |            |            |
| SAVE_TO_IMS_ES_INDEX               | 30         | 48         | 488        | 462        |            |            |            |
| SAVE_TO_SEARCH_STORAGE             | 275        | 29         | 531        | 24         |            |            |            |
| total work flow                    | 329        | 97         | 1026       | 516        | 52         | 36         | 71         |
|                                    |            |            |            |            |            |            |            |

