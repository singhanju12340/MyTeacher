
### Next best action 
For acquiring account.




#### Request of account-summary  API

```Java
{
	"action" : "account-summary/ engagement / intent/ technographic/ activities",
	"tenantId" : "tenantId",
	"id" : "id"
}
```



#### Cache keys
```
sales-ai-stage-redis-cluster-001.jukoeb.0001.use1.cache.amazonaws.com:6379> keys *

1) "summaryResponses::ActionRequest(action=engagement, tenantId=1681, id=471404)"

2) "summaryResponses::ActionRequest(action=technographic, tenantId=1681, id=471404)"

3) "summaryResponses::ActionRequest(action=intent, tenantId=1681, id=471404)"

4) "summaryResponses::ActionRequest(action=account-summary, tenantId=1681, id=471404)"

5) "summaryResponses::ActionRequest(action=activities, tenantId=1681, id=471404)
```

#### Engagement Keys data
``` 
{
  "conclusion": [
    "The account **atile.branding** has shown **no engagement** over the last 30 days and 3 months...",
    "There have been **no sales touches** in the last 14 days...",
    "There is **no trending onsite engagement**...",
    "Based on these statistics, it appears that **atile.branding** is currently an **inactive account**..."
  ],
  "summaryDataPoints": [
    "0 web visits",
    "0 engagement minutes",
    "no sales touches",
    "no people engaged"
  ]
}
```



### Model selection decision:

Collaborative Filtering
Reinforcement Learning
Classification Models


Hybrid Models / Ensemble Methods

Deep Learning (e.g., Recurrent Neural Networks - RNNs, Transformers):

Given the goal of generating "next best action" recommendations to sales teams based on account engagements, insights, and campaign responses to convert leads into deals, a **hybrid approach leveraging Gradient Boosting Machines (like XGBoost or LightGBM) as the core classification/ranking engine, augmented with feature engineering from time-series data and potentially NLP for unstructured insights, is the most suitable starting point.**