---
Creation Time: Tuesday, February 11th 2025
Modified Time: Tuesday, February 11th 2025
---
### [Functional Requirements](https://www.hellointerview.com/learn/system-design/in-a-hurry/delivery#1-functional-requirements)

**Core Requirements**
1. Users should be able to create posts.
2. Users should be able to friend/follow people.
3. Users should be able to view a feed of posts from people they follow, in chronological order.
4. Users should be able to page through their feed.
    
**Below the line (out of scope):**
- Users should be able to like and comment on posts.
- Posts can be private or have restricted visibility.

### [Non-Functional Requirements](https://www.hellointerview.com/learn/system-design/in-a-hurry/delivery#2-non-functional-requirements)
**Core Requirements**
1. The system should be highly available (prioritizing availability over consistency). Tolerate up to 2 minutes for eventual consistency.
2. Posting and viewing the feed should be fast, returning in < 500ms.
3. The system should be able to handle a massive number of users (2B).
4. Users should be able to follow an unlimited number of users, users should be able to be followed by an unlimited number of users.