---
Creation Time: Thursday, April 3rd 2025
Modified Time: Thursday, April 3rd 2025
---
Design a product recommendation system for Target.com that can handle millions of users and a vast product catalog, considering real-time recommendations based on the current shopping session.


## FR:
Provide recommendation based on user shopping/watch, card, browsing history
Recommendation should be provided based on recent activity as well in real time within Sec.
Provide Millions of concurrent users and vast product catalog 
Support multichannel recommendations web, device

## NFR
Scale horizontally to handle high request volume and large data sets.
Real-time recommendations must be computed quickly (sub-second response times).
The system should tolerate node failures and maintain uptime.
 Ensure recommendations reflect the current session and recent user behavior.


What does user wants as recommendations?
how do we scale it?



