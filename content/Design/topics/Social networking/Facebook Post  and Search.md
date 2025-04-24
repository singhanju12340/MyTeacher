---
Creation Time: Thursday, April 10th 2025
Modified Time: Thursday, April 17th 2025
---
This question is primed for infrastructure-style interviews where your interviewer wants to understand how deeply you understand data layout, indexing, and scaling.

We're going to assume our interviewer has explicitly told us that **we're not allowed to use a search engine like Elasticsearch or a pre-built full-text index** (like Postgres Full-Text) for this problem.


**Core Requirements**
1. Users should be able to create and like posts.
2. Users should be able to search posts by keyword.
3. Users should be able to get search results sorted by recency or like count.

**Below the line (out of scope)**
- Support fuzzy matching on terms (e.g. search for “bird” matches “ostrich”).
- Personalization in search results (i.e. the results depend on details of the user searching).
- Privacy rules and filters.
- Sophisticated relevance algorithms for ranking.
- Images and media.
- Realtime updates to the search page as new posts come in.