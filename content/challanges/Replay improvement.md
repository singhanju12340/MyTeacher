---
Creation Time: Tuesday, November 5th 2024
Modified Time: Thursday, November 28th 2024
---
**Preprod**
Total Events: 10 lakhs, 
25 partition
gm_0_0_0_aec_cp_search_inventory_v7 2 2 66.6gb
gm_0_0_0_aec_cp_ims_inventory_v3 2 2 93.3gb
5 concurrent consumer

**Prod**
Total Events: 40 lakhs on prod 
gm_0_0_0_aec_cp_search_inventory_v1  121.9gb
gm_0_0_0_aec_cp_ims_inventory_v1 611.1gb

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

**Current time taken matrix for preprod 10 lakh events**
Took 3 hours to run

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

**Current time taken matrix for preprod 20 lakh events**
triggered at 12 AM

|                                    |            |            |            |            |            |     |     |
| ---------------------------------- | ---------- | ---------- | ---------- | ---------- | ---------- | --- | --- |
| Task name                          | time in ms | time in ms | time in ms | time in ms | time in ms |     |     |
| VALIDATE                           | 3          | 1          |            | 1          |            |     |     |
| FETCH_DC_DEALER_DETAILS            | 0          |            |            | 0          |            |     |     |
| IMS_DATA_DOWNLOAD_FROM_STORAGE     |            | 18         | 6          | 18         |            |     |     |
| UPLOAD_TO_STORAGE                  | 54         |            |            |            | 39         | 16  | 45  |
| SAVE_TO_DB                         | 8          |            |            |            | 9          | 9   |     |
| TRANSFORM_TO_SEARCH_ES_PAYLOAD     | 0          |            |            | 0          |            |     |     |
| TRANSFORM_TO_IMS_ES_PAYLOAD        | 1          | 1          |            | 1          | 1          |     |     |
| ADD_CS_DEALER_DETAILS_IN_SEARCH_ES | 0          |            |            | 0          | 1          |     |     |
| SAVE_TO_IMS_ES_INDEX               | 839        | 48         | 488        | 54         | 18         | 28  | 487 |
| SAVE_TO_SEARCH_STORAGE             | 25         | 29         | 531        | 27         | 17         | 22  | 350 |
| total work flow                    | 935        | 100        | 1026       | 102        | 89         | 93  | 942 |
|                                    |            |            |            |            |            |     |     |
|                                    |            |            |            |            |            |     |     |
|                                    |            |            |            |            |            |     |     |
|                                    |            |            |            |            |            |     |     |
|                                    |            |            |            |            |            |     |     |

### Problems:
All network integrations was processed as a single event. 
1. such as kafka consumer was consuming single event at a time
2. Single vin was getting saved at a time in  ES, too much overloaded network calls to ES
3. Replay trigger was fetch data from mongo in  batch, so producer was able to push 20 lakh vins with in 10 min
4. Blob interaction was also done as a single


### Solution
First POC for batch ES interaction
1. change kafka consumer batch consumer
2. Query mongo in bluk
3. Save ES in bulk
4. Fetch and save blob in bulk
5. Moved Fetch dealer details from single to batch api 


In memory consumption in batch processing
```
>  batch:100

> concurrent consumer:5

> 500 * 16/17KB data per vin = 7500 KB ~ **7.5 MB** for TInventory blob object data.

> Currently its only every

> 5 * 16 KB
```


### 2nd solution 
Batch save to ES as a separate async task
Send transformed message payload via kafka
`elastic max data size/ max record can be saved in bulk
	100 mb or 10000
	
`kafka disk uses
1 vin transformed data: 14KB
1 KB = 1/14 vin data
1 * 1012 * 1012 = 6*6000000 / 14 ~ 6 * 428000 approx vin at one time

2568000 vin will consume 6 gb space

for 40 lakh vins to be replayed, we will need 24 GB data broker storage on preprod, with retention period of this topic as 3 hours
need to run and test the outcome


### 3rd Solution
Send list of download url for blob, download a list. transform, then save in ES in bulk>> bad idea

GC setting for batch processing, using parallel GC: -XX:+UseParallelGC


gm_0_0_0_private_aec_inventory

public_onboarding_csco_topic
public_drp_dealer_update

gm_0_0_0_public_int_dealer_purchase_status_outbound
gm_0_0_0_public_int_dealer_purchase_status_inbound


gm_chevrolet_0_0_private_aec_dealer_update
gm_cadillac_0_0_private_aec_dealer_update
