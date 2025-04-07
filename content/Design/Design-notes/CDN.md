---
Creation Time: Monday, June 24th 2024
Modified Time: Thursday, April 3rd 2025
---
Content delivery network or Edge Server
It helps in better loading speed
stores and loads static content at better loading speed
reduces bandwidth cost as it reduces server calls 
improves SEO

_Advantages:
1. periodically invalidates cache from original server content
2. Helps in SEO (Search Engine Optimazations)
3. provides HTTPS connections
4. SSL(Secure Socket Layer) can be enables to provide cross site security
5. It prevents DDos(Distribution Denail of Service) attack


Amazone: Amazon CDN Cloud front
Akamai CDN
Microsoft CDN
Cloudfare

_How cloudfareworks:
### The Cloudflare Backbone_

It comprises a network of long-distance fiber optic cables connecting various Cloudflare data centers across North America, South America, Europe, and Asia. This also includes Cloudflare’s metro fiber network, directly connecting data centers within a metropolitan area.
Our backbone is a dedicated network, providing guaranteed network capacity and consistent latency between various locations. It gives us the ability to securely, reliably, and quickly route packets between our data centers, without having to rely on other networks.
This dedicated network can be thought of as a fast lane on a busy highway. When traffic in the normal lanes of the highway encounter slowdowns from congestion and accidents, vehicles can make use of a fast lane to bypass the traffic and get to their destination on time.


#### Push-based vs. Pull-based CDN

_**PUSH **_ 

content is explicitly "pushed" from the origin server or content provider to CDN's cache server.
APIs, FTP, or other file transfer mechanisms to upload files to the CDN servers
Content is pre-populated on the CDN, ensuring it’s available for users immediately after deployment
_The content provider has complete control over what is cached and when updates occur_
_here is a predictable pattern of updates and content consistency across the CDN nodes_
_Works well for content that rarely changes, such as software downloads, videos, or static web assets_
`Requires manual or scheduled processes to push content updates to all CDN nodes`
`Propagating new or updated content to all nodes may introduce delays before the new version is available globally`
`Pushing large volumes of content, especially when updates are frequent, can consume significant bandwidth and processing resources`

Suitable for content that updates infrequently
used by companies that need strict control over when content is updated e.g., enterprise software downloads


_**PULL**_

The CDN servers automatically retrieve content from the origin server the first time it is requested
A resource that is not present in the CDN’s cache, the CDN pulls the content from the origin server, caches it, and then serves it to the user
Cached content is typically set to expire based on a Time-to-Live (TTL) or cache-control headers. Once expired, subsequent requests trigger a fresh pull from the origin
_Content providers don't need to manually manage cache population_
_Only requested content is cached_
_The CDN acts as a proxy for all content, so any updates on the origin are reflected after the TTL expires, without needing manual intervention_
`The first request for a resource not yet cached may be slower`
`Can cause thunder heard problem, If many requests hit the origin for uncached content`

Ideal for dynamic or frequently updated content since it automatically pulls the latest version from the origin
Used by websites, blogs, and eCommerce platforms where content changes regularly and the convenience of automatic caching outweighs the initial request latency




