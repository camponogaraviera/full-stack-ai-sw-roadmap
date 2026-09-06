<div align='center'>
    <h1> 3.5 System Design </h1>
    <h2> 3.5.8 The Hot Spot (Celebrity) Problem </h2>
</div>

# About

This is a problem of high throughput that happens when there are too many requests (high traffic) to a given shard, causing it to overload and crash. For example, a viral video on YouTube, a trending topic on Twitter (e.g., Super Bowl), or a public figure/celebrity profile on Instagram.

It does not always bring the whole application down, but if it overwhelms shared infrastructure (e.g., database, cache, or a partition), it can cause cascading failures that impact the entire system.

- Solution for a viral video: temporarily store the video in a CDN.

- Solution for Superbowl or profile page: provisioning an individual compute instance (to handle peak traffic) with a Hot Standby (to ensure high availability) and sufficient Read Capacity Units (to avoid Throttling), along with an autoscale feature (for automation).

A smart caching solution can detect the source of higher traffic and cache responses from there.

Note: throttling is a mechanism that limits the rate of requests to a system to prevent resource exhaustion and maintain stability and availability. In DynamoDB, throttling occurs when read/write requests exceed the provisioned or burst capacity.
