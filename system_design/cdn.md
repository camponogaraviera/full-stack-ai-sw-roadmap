<div align='center'>
    <h1> 3.5 System Design </h1>
    <h2> 3.5.7 Content Delivery Network (CDN) </h2>
</div>

# About

A Content Delivery Network (CDN) is a network of servers spread across the globe with the primary purpose of reducing the load on origin servers and data stores. It is also used to minimize client latency (time to render the page) by delivering static files from geographically closer edge servers, thus reducing the need for requests to traverse long distances to the origin server. **It can get very expensive as the system scales.**

CloudFront serves content (HTML, CSS, JS, images, fonts) for static websites when connected to an origin like S3, but cannot host files/websites by itself.

## Use cases

1. If a user from Taiwan wants to watch a Korean movie that was originally stored on a server in Korea, the system will fetch a copy of this movie from a geographically closer available server (e.g., a server in Taiwan).

2. Every time a user clicks to watch a video on a platform, there is a cost associated with transferring the video file from cloud storage (like AWS S3) to the user. To reduce these costs, e-learning platforms often cache frequently accessed videos using a CDN. The cached video does not disappear when a user logs out, because CDN caching is based on the content URL and the cache's TTL (time-to-live), not on individual user sessions. The cached copy remains at the CDN edge location until it either expires (based on the TTL, which defaults to 24 hours unless configured otherwise) or is evicted due to low demand or space constraints. If that happens, the CDN will fetch the video from S3 again on the next request, and the platform incurs the data transfer cost again
