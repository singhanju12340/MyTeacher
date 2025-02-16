---
Creation Time: Monday, September 16th 2024
Modified Time: Monday, September 16th 2024
---

#### **Functional Requirement**

- Provide inventory data for consumer portal search.
- Provide inventory data by API for VDP/OMS/Myaccount
- Provide es index resync capability in case of es index schema change or elastic outage.
- Source of truth for consumer portal inventory.
#### **Non-functional Requirement**
- 300 events per second for one program. 1k/second for Bolt, Carbravo, and Cadillac
- Scalability
- Read heavy for IMS APIs
- Write Heavy for inventory consumption and ingestion.