---
Creation Time: Wednesday, April 9th 2025
Modified Time: Wednesday, April 9th 2025
---
GET gm_0_0_0_aec_cp_ims_inventory/_count
{
  "query": {
   "bool": {
     "filter": [
       {
         "term": {
           "availability.deleted": {
             "value": false
           }
         }
       },
       {
         "term": {
           "availability.isAvailable": {
             "value": true
           }
         }
       },
       {
         "range": {
           "metaData.imsUpdateTimestamp": {
             "gte": 1744050705000         
             }
         }
       }
     ]
   }
  }
}