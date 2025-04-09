---
Creation Time: Wednesday, April 9th 2025
Modified Time: Wednesday, April 9th 2025
---
## Fields to be considered

**Can be modified based on templates from product.

1. Make
2. Model
3. Year
4. Dealer
5. Pricing
6. ExtColor

## Template
All suggestions will be defined as a template persisted in a mongo collection which will be parsed accordingly with the possible values of various attributes and reindexed in elastic index. For each kind of suggestion a template will be defined with placeholders for various attributes. The template formats would be in accordance with the product team. Example:

`<bodyType> Cars`
`<bodyType> less than <price>`
`<color> <bodyType> less than <price>`

```
templateId : f67a,
templateName : <bodyType> less than <price>,
filters : [
	  {
	     field : bodyType,
	     value : <bodyType>,
	     operator : in
	  },
	  {
	     field : price,
	     value : <price>,
	     operator : lte
		
	  }
	]
}]
imgUrl: https://imgurl
isImg: true
tenantId: gm_chevrolet
```
### Sample Templates

```
 [
            {
                "id": "64ecfa226ca656280d82df45",
                "templateName": "${PROGRAM_TIER}",
                "termFields": [
                    "PROGRAM_TIER"
                ],
                "oemId": "HONDA",
                "programId": "ACURA_CPO",
                "dealerId": "0",
                "tenantId": "0"
            },
            {
                "id": "64ecfa226ca656280d82df4d",
                "templateName": "Vehicles between ${CASH_START} and ${CASH_END}",
                "rangeFields": {
                    "name": "CASH",
                    "values": [
                        {
                            "min": 10000,
                            "max": 20000
                        },
                        {
                            "min": 10000,
                            "max": 30000
                        },
                        {
                            "min": 20000,
                            "max": 30000
                        },
                        {
                            "min": 20000,
                            "max": 50000
                        },
                        {
                            "min": 30000,
                            "max": 50000
                        },
                        {
                            "min": 40000,
                            "max": 50000
                        }
                    ]
                },
                "oemId": "HONDA",
                "programId": "ACURA_CPO",
                "dealerId": "0",
                "tenantId": "0"
            }
```

![[Screenshot 2025-04-09 at 12.50.55 PM.png]]

### API
```
https://www.carbravo.com/crb/drp-cp-api/p/v2/auto-suggestions

request:

{
  "input": "sed",
  "program": "CARBRAVO_AUTOSUGGEST",
  "resultSize": 10
}


Response: 

{
    "data": {
        "status": "success",
        "data": {
            "textSuggestions": [
                {
                    "displayText": "Sedan with Third Row Seat",
                    "templateId": "239595718",
                    "searchFields": "Sedan with Third Row Seat",
                    "program": "CARBRAVO_AUTOSUGGEST",
                    "filters": {
                        "bodyType": {
                            "values": [
                                "Sedan",
                                "Car"
                            ],
                            "operator": "IN"
                        },
                        "utilityFeatures": {
                            "values": [
                                "13740"
                            ],
                            "operator": "IN"
                        }
                    },
                    "img": false
                },
                {
                    "displayText": "Sedan",
                    "templateId": "599734025",
                    "searchFields": "Sedan",
                    "program": "CARBRAVO_AUTOSUGGEST",
                    "filters": {
                        "bodyType": {
                            "values": [
                                "Sedan",
                                "Car"
                            ],
                            "operator": "IN"
                        }
                    },
                    "img": false
                },
                {
                    "displayText": "Sedan with Heated Seats",
                    "templateId": "239595718",
                    "searchFields": "Sedan with Heated Seats",
                    "program": "CARBRAVO_AUTOSUGGEST",
                    "filters": {
                        "bodyType": {
                            "values": [
                                "Sedan",
                                "Car"
                            ],
                            "operator": "IN"
                        },
                        "premiumFeatures": {
                            "values": [
                                "13310"
                            ],
                            "operator": "IN"
                        }
                    },
                    "img": false
                }
            ]
        }
    },
    "status": "success"
}

```

```
{
  "input": "1000",
  "program": "CARBRAVO_AUTOSUGGEST",
  "resultSize": 10
}

{
    "data": {
        "status": "success",
        "data": {
            "textSuggestions": [
                {
                    "displayText": "Audi with less than 1,000 miles",
                    "templateId": "1877196074",
                    "searchFields": "Audi with less than 1000 miles",
                    "program": "CARBRAVO_AUTOSUGGEST",
                    "filters": {
                        "make": {
                            "values": [
                                "Audi"
                            ],
                            "operator": "IN"
                        },
                        "mileage": {
                            "max": 1000.0
                        }
                    },
                    "img": false
                },
                {
                    "displayText": "GMC with less than 10,000 miles",
                    "templateId": "1877196074",
                    "searchFields": "GMC with less than 10000 miles",
                    "program": "CARBRAVO_AUTOSUGGEST",
                    "filters": {
                        "make": {
                            "values": [
                                "GMC"
                            ],
                            "operator": "IN"
                        },
                        "mileage": {
                            "max": 10000.0
                        }
                    },
                    "img": false
                }
            ]
        }
    },
    "status": "success"
}
```


Auto Suggestion template
```

```

### AutoSuggestion Ingestion: How Autosuggestion data is getting created?
![[Screenshot 2025-04-09 at 12.43.52 PM.png]]
### Fetch Autosuggestion

![[Screenshot 2025-04-09 at 12.44.44 PM.png]]