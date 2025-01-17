---
Creation Time: Thursday, July 25th 2024
Modified Time: Friday, January 10th 2025
---
- Ability to upload videos fast
    
- Smooth video streaming
    
- Ability to change video quality
    
- Low infrastructure cost
    
- High availability, scalability, and reliability requirements
    
- Clients supported: mobile apps, web browser, and smart TV

#### Functional requirement

_User management
1. User should be able to register and login
2. Users can have different levels of subscriptions

_Content management:
1. upload videos
2. Different levels of views like public or only with closed groups
3. Play video on different resolution, it should support Mobile apps, web browsers, and smart TV.
4. User can comment, like and share videos and add videos to playlist
5. Video upload supports almost all video resoultion
6. File size limit: small and medium videos around 1GB
7. 


_Search Management:
1. Provide text based search


_Recommendations:_
1. provide list of videos based on the user interest, watched history



_Estimates:_
DAU: 10 M
Avg Day time spent: 30 min
Avg Video size : 300MB, Max - 1GB
User watches 5 videos per day
10 % of 10 M upload 1 video per day

Daily storage increase :: 300MB * (10% 10M) = 3GB * 10%* 1M = 3 * 100000 = 300TB

CDN charges: lets use Amazon cloud front CDN
Avg CDN cost per GB = 0.02$/ GB
CDN cost per day = 5 videos * (0.02 * 300 / 1000 GB) * 10M $ = $ 0.3 M  


Lets focus on 2 major flow
1. upload video
2. stream video

This system is divided into 2 parts. 1 streaming server which handles streaming of videos. 2nd API server which handles video search, recommendations, user registration, metadata upload, generating video upload URL.



## Video Thumbnails

Small size video 5KB. 
On the dashboard, multiple thumbnails will be shown to users. and fetching them from disk is inefficient as different thumbnails will be stored at different locations in the disk. This can cause high latency. This problem can be solved by using _BigTable_ as it combines multiple thumbnails and store it at single place in disk.

Keeping trending thumbnails in cache will help in improving read latency and because of its small size we can easily cache multiple thumbnails into a cache.

#### Metadata Sharding

Different ways to shard video metadata as there are huge number of videos uploaded daily in youtube.

_Shard based on userId_ : 
This approach has a couple of issues:

1. What if a user becomes popular? There could be a lot of queries on the server holding that user; this could create a performance bottleneck. This will also affect the overall performance of our service.
2. Over time, some users can end up storing a lot of videos compared to others. Maintaining a uniform distribution of growing user data is quite tricky.

_Sharding based on VideoID:_ Our hash function will map each VideoID to a random server where we will store that Video’s metadata. To find videos of a user, we will query all servers, and each server will return a set of videos. A centralized server will aggregate and rank these results before returning them to the user. This approach solves our problem of popular users but shifts it to popular videos.

We can further improve our performance by introducing a cache to store hot videos in front of the database servers.

_Sharded and Distributed Caching: __
For high-scale systems, a single cache instance might become a bottleneck. Use **sharded caches** to distribute the load:

- **Partition by Video ID**:
    - Divide videos into shards using consistent hashing.
    - Each Redis instance handles a subset of video metadata.

**Example**:

- Redis Instance 1: Handles `video_id` 1–1000.
- Redis Instance 2: Handles `video_id` 1001–2000.

_**Video deduplications_ : 
Duplicate videos often differ in aspect ratios or encodings, contain overlays or additional borders, or be excerpts from a longer original video. The proliferation of duplicate videos can have an impact on many levels:

There are muitple algorithms to detect duplication. 
videos needs to be divided into small segments and can be compared based on different video and audio segment.
**Block Matching** is a technique widely used in computer vision and video processing, particularly for motion estimation, video compression, and similarity detection. The goal is to find a block (or region) in one frame of a video that best matches a corresponding block in another frame.

If the newly uploaded video is a subpart of an existing video or vice versa, we can intelligently divide the video into smaller chunks so that we only upload the parts that are missing.






















