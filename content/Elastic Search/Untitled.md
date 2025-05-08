---
Creation Time: Monday, August 5th 2024
Modified Time: Thursday, May 8th 2025
---
GET _search
{
  "query": {
    "match_all": {}
  }
}

GET gm_0_0_0_aec_cp_search_inventory/_search
{
  "size": 100,
  "query": {
    "bool": {
      "must": [
        {
          "terms": {
            "metadata.programIds.keyword": [
              "CHEVROLET"
            ]
          }
        },
        {
          "term": {
            "availability.tekionStatus.code": {
              "value": "BOOKED"
            }
          }
        },
        {
          "terms": {
            "stock.type": [
              "DealerStock"
            ]
          }
        }
      ]
    }
  }
}

GET gm_0_0_0_aec_cp_ims_inventory/_search
{
  "size": 0,
  "query": {
    "bool": {
      "must": [
        {
          "terms": {
            "dealers.dealers.state": [
              " FL"
            ]
          }
        },
        {
          "terms": {
            "metadata.programIds.keyword": [
              "CHEVROLET"
            ]
          }
        },
        {
          "term": {
            "availability.deleted": {
              "value": "false"
            }
          }
        },
        {
          "term": {
            "availability.display": {
              "value": "true"
            }
          }
        },
        {
          "terms": {
            "stock.type": [
              "DealerStock"
            ]
          }
        }
      ]
    }
  },
  "aggs": {
    "dealers": {
      "terms": {
        "field": "dealers.parentDealer.bac",
        "size": 100
      }
    }
  }
}


GET gm_0_0_0_aec_cp_search_inventory
{
  "_source": ["vin"], 
  "query": {
    "term": {
      "dealers.parentDealer.bac": {
        "value": "198917"
      }
    }
  }
}




GET gm_0_0_0_aec_cp_search_inventory_v3/_count
{
  "_source": ["vin", "metadata.updatedTimestamp"], 
  "sort": [
    {
      "metadata.updatedTimestamp": {
        "order": "asc"
      }
    }
  ]
}

GET _cat/allocation?v



GET gm_chevrolet_0_0_aec_cp_central_stock_dealers/_search
{
  "_source": ["metadata.dealerId"], 
  "query": {
    "match": {
      "distributionCenters": "326961"
    }
  }
}


GET gm_0_0_0_aec_cp_ims_inventory/_search
{
  "_source": ["stock.type"], 
  "query": 
  {
    "bool": {
      "must": [
        {
          "term": {
          "dealers.logisticCenter.": {
          "value": "320957"
          }
        }
          
        }
      ]
    }
    
  }
}

GET gm_chevrolet_0_0_aec_cp_central_stock_dealers/_search
{
  "size": 0,  
  "aggs": {
    "tes": {
      "terms": {
        "field": "distributionCenters",
        "size": 10
      }
    }
  }
}

GET gm_0_0_0_aec_cp_ims_inventory/_search
{
  "_source": ["vin", "dealers.logisticCenter.bac"],  
  "query": {
    "bool": {
      "must": [
        {
          "term": {
      "stock.type": {
        "value": "CentralStock"
      }
    }
        },
        {
          "term": {
      "dealers.logisticCenter.bac": {
        "value": "326961"
      }
    }
        }
      ]
    }
  }
}




GET gm_0_0_0_aec_cp_ims_inventory/_search
{
  "_source": ["vin", "dealers.logisticCenter.bac"],  
"query": {
    "term": {
      "stock.type": {
        "value": "CentralStock"
      }
    }
  }, 
  "aggs": {
    "NAME": {
      "terms": {
        "field": "dealers.logisticCenter.bac",
        "size": 10000
      }
    }
  }
}

GET gm_0_0_0_aec_cp_search_inventory/_mapping

GET gm_0_0_0_aec_cp_search_inventory/_search
{
    "query": {
        "match": {
            "vin": "2F"
        }
    },
    "sort": [
        {"inventoryVersion": "asc"},
        {"tie_breaker_id": "asc"}      
    ]
}



GET gm_0_0_0_aec_cp_search_inventory/_search
{
  "query": {
   "bool": {
      "must": [
      {
       "terms": {
            "metadata.programIds.keyword": [
              "CHEVROLET"
            ]
          }
    },
    {
       "terms": {
            "metadata.programIds.keyword": [
              "CADILLAC"
            ]
          }
    },
     {
       "term": {
            "stock.condition": "NEW"
          }
    },
     {
       "term": {
            "metadata.evenType": "listed"
          }
    },
    {
      "terms": {
            "pricing.incentives.cash.name": [
              
              "CHEVROLET"
            ]
          }
    }
   ]   
   }   
  }
}





GET gm_0_0_0_aec_cp_search_inventory/_count
{
  "query": {
    "bool": {
      "must": [
        {
          "terms": {
            "stock.condition": ["CARBRAVO"]
          }
        },
        {
          "script": {
            "script": {
              "source": "doc['metadata.programIds.keyword'].size() > 0",
              "lang": "painless"
            }
          }
        }
      ]
    }
  }
}


GET gm_0_0_0_aec_cp_search_inventory/_count
{
  "query": {
   "bool": {
      "must": [
      {
       "terms": {
            "metadata.programIds.keyword": [
              "GMC",
              "BUICK",
              "CADILLAC",
              "CHEVROLET"
            ]
          }
    } ,
     {
       "terms": {
            "stock.condition": ["NEW", "USED", "CPO", "CARBRAVO_CERTIFIED"]
          }
    },
    {
      "term": {
        "availability.deleted": {
          "value": false
        }
      }
    }
   ]   
   }   
  }
}



GET gm_0_0_0_aec_cp_search_inventory/_search
{
  "query": {
   "bool": {
      "must": [
      {
       "terms": {
            "metadata.programIds.keyword": [
              "GMC",
              "BUICK"
            ]
          }
    } ,
     {
       "term": {
            "stock.condition": "NEW"
          }
    }
   ]   
   }   
  },
  "aggs": {
    "t": {
      "terms": {
        "field": "programIds",
        "size": 10
      }
    }
  }
}












GET gm_0_0_0_aec_cp_search_inventory/_alias







GET gm_0_0_0_aec_cp_search_inventory/_search
{
  "size": 0, 
  "aggs": {
    "test": {
      "terms": {
        "field": "availability.tekionStatus.name",
        "size": 10
      }
    }
  }
}




GET gm_0_0_0_aec_cp_search_inventory/_search?size=1000
{
  "_source": ["vin"], 
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "availability.inventoryStatus.code": "FCItCSRtlTGM"
          }
        },
        {
          "term": {
            "availability.tekionStatus.code": "AVAILABLE_NOW"
          }
        }
      ]
    }
  },
  "sort": [
    {
      "metadata.updatedTimestamp": {
        "order": "desc"
      }
    }
  ]
}







GET _nodes/stats/thread_pool








GET gm_0_0_0_aec_cp_search_inventory/_search
{
  "query": {
    "terms": {
      "vin": [
        "3MW39FS00P8C94581",
"1FMSK8BHXMGC06987",
"3GCPDBEK7RG304427",
"KL4CJASB9MB301014",
"LRBFXCSA8KD143544",
"1GKKNUL47PZ235280"
      ]
    }
  }
}




GET gm_cadillac_0_0_aec_cp_search_inventory/_search?size=1000
{
  "_source": ["availability"], 
  "query": { 
    "bool": { 
      "filter": [ 
        {
          "term": {
          "availability.inventoryStatus.name": "AVAILABLE_NOW",
          "availability.inventoryStatus.code": "VH_Rtl_Stk"
        }  
        }
      ]
    }
  }
}





