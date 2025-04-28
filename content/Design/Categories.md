---
Creation Time: Friday, April 25th 2025
Modified Time: Friday, April 25th 2025
---
I've categorized them based on themes + added notes on what each one really tests you on.  
  
► Read-Heavy Systems  
	These focus on scale, latency, and efficient data fetching.  
	1. Design a URL Shortener (Bitly)  
	→ Talk about key generation, collisions, and DB storage.  
	→ Add caching and DB sharding if traffic is high.  
	  
	2. Design an Image Hosting Service  
	→ Talk about object storage (S3, GCS) + CDN usage.  
	→ Consider image deduplication and resizing strategies.  
	  
	3. Design a Social Media Platform (Twitter/Facebook)  
	→ Talk about posts, timelines, relationships (follows, friends).  
	→ Focus on denormalized storage and sharding.  
	  
	4. Design a NewsFeed System (Hard)  
	→ Push vs Pull models, Fanout on Write vs Read.  
	→ Caching, pagination, and ranking algorithms.  
  
► Write-Heavy Systems  
	Here, durability, throughput, and ingestion speed are critical.  
	  
	1. Design a Rate Limiter  
	→ Token bucket or leaky bucket algorithms.  
	→ Redis-backed counters + TTL logic.  
	  
	2. Design a Log Collection and Analysis System  
	→ Use Kafka for ingestion, and something like ELK for processing.  
	→ Talk about partitioning, buffering, and real-time querying.  
	  
	3. Design a Voting System  
	→ Idempotency, fraud prevention, and result aggregation.  
	→ Real-time vs eventual vote count updates.  
	  
	4. Design a Trending Topics System  
	→ Use count-min sketch or approximate counting.  
	→ Talk about sliding window aggregation + ranking.  
  
► Strong Consistency Systems  
	Transactional integrity and failure handling become the focus.  
	  
	1. Design an Online Ticket Booking System  
	→ Handle race conditions with locking or optimistic concurrency.  
	→ Talk about seat reservation + payment flow.  
	  
	2. Design an E-Commerce Website (Amazon)  
	→ Cover product catalog, cart service, order processing.  
	→ Include DB consistency, checkout idempotency.  
	  
	3. Design an Online Messaging App (WhatsApp/Slack)  
	→ Talk about message queues, delivery receipts, retries.  
	→ Offline storage, notification delivery, scaling chat infra.  
	  
	4. Design a Task Management Tool  
	→ CRUD APIs, user auth, task assignment.  
	→ Background jobs, status updates, and audit trails.  
  
► Scheduler Services  
	Timing, reliability, and eventual execution are tested here.  
	  
	5. Design a Web Crawler  
	→ BFS vs DFS for crawling, politeness rules.  
	→ Distributed queues, duplicate URL filters.  
	  
	6. Design a Task Scheduler  
	→ Job queues, retry logic, cron-based triggers.  
	→ Priority queues and task deduplication.  
	  
	7. Design a Real-Time Notification System  
	→ Push vs Polling, webhooks, and device token mgmt.  
	→ Scale delivery across millions of users.



► Trie / Proximity Systems  
	Efficient data structures and latency-optimized retrieval.  
	  
	1. Design a Search Autocomplete System  
	→ Trie or Ternary Search Tree backed by frequency rank.  
	→ Debouncing, caching, and typo-tolerance.  
	  
	2. Design a Ride-Sharing App (Uber/Lyft)  
	→ Matchmaking engine, real-time location tracking.  
	→ Talk about ETA algorithms, surge pricing, DB design.