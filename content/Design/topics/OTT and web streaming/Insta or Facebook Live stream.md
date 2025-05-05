---
Creation Time: Thursday, January 9th 2025
Modified Time: Tuesday, April 29th 2025
---
 Streaming of a live  on the scale of 10M watching together.



Design : [[Live Streaming]]


 `RTMP` protocol  to send live video from the broadcaster to the Facebook infrastructure. It’s based on TCP and splits the live video into separate audio and video streams.
 Unlike `HLS`, RTMP uses a push model. Instead of the player requesting each segment, the server continuously sends video and audio data.

`Transcoding`
Live stream server used to transcode live video into different bit rates.

`DASH` is a streaming protocol over HTTP. It consists of a manifest file and media files. Imagine the manifest file as a collection of pointers to media files. They update the manifest file whenever new video segments are created. Hence the viewer can request the media files in the correct order.


### How FB handled scaling:

1. `Consistent hashing` for Live streaming servers. used stream ID as the consistent hash key
	
2. Network bandwidth: Used `ABR adaptive bitrate streaming`. A live video gets broken down into smaller parts. And each part gets converted into different resolutions.
3. Used `Edge and Origin server` closer to viewers to store video segments.
An edge server is **specialized hardware designed to aggregate the data locally**, reduce bandwidth and enable shorter travel of a request. It can server 200K/Sec
when a viewer requests a video segment, they serve it from the edge server if it got cached. Otherwise the request gets forwarded to the origin server and live stream server.

4. `Thundering herd:
They added Http proxy server and cache in edge server. When fewer request was cache miss from edge cache. HTTP proxy forwarded request to origin server and if missed on origin server it was sent to origin Live streaming server. 
Cache on Edge and Origin was distributed, to balance the load on cache servers.

Adding only edge and origin server dit not help with thernder herd issue, about 1.8 percent of requests were getting past the edge cache. Then they used request coalescing.

==**request coalescing** Queues were created at Edge to buffer all concurrent request of specific same video segment which cahce missed. and only 1 request was sent  to parent server(origin or LS) to fetch details. It reduced load on parent server and avoided thunder herd. All request in the queue were served after the response was updated in the cache. Same implementation was replicated on Edge and Origin server.
Request Coalescing  upon receiving a request, first, open a mutex lock. Then, the Request Coalescing system will send a single request to the origin. As soon as the origin responds, the mutex is released and all the waiting connections are automatically connected to the origin response.
The short answer is no. Request Coalescing does not assure only a single request will ever be sent to the origin. It only combines uncached requests that are accessing a single resource at the same time. With Request Coalescing enabled, your origin could still receive a request to the same resource if the requests arrive sequentially

5. `Encoding:` Video encoding can be done at both Client or server
The trade-off between encoding on the client-side and the server-side is mostly around video quality vs. latency and reliability.
Since encoding is typically lossy if the network is good the Facebook app will try to keep the quality as high as possible and use little or no encoding. 
Conversely if the network is less good then more encoding is done on the phone to keep the amount of data to be transferred smaller.



https://www.infoq.com/podcasts/sachin-kulkarni-facebook-live/





