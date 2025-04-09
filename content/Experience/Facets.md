---
Creation Time: Wednesday, April 9th 2025
Modified Time: Wednesday, April 9th 2025
---
```
https://www.carbravo.com/crb/drp-cp-api/p/v2/vehicles/facets

Response:


```
![[Screenshot 2025-04-09 at 12.06.46 PM.png]]



Facets request with filter params
```
{
  "filters": {
    "finance": {
      "downPayment": 2000
    },
    "geo": {
      "zipCode": "95101",
      "radius": 250
    },
    "bodyType": {
      "values": [
        "SUV",
        "Crossover"
      ]
    },
    "searchText": ""
  }
}
```
![[Screenshot 2025-04-09 at 12.17.28 PM.png]]