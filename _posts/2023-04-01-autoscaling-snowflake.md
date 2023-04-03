---
title: Autoscaling in Snowflake
tags: [snowflake, datawarehouse, architecture, auto-scaling]
style: fill
color: warning
description: How caching works in the snowflake data warehouse
---
The workloads in the Snowflake data warehouse are varied and can be unpredictable. It is difficult for customers to predict when they will need more compute resources, especially during peak usage times. This is where autoscaling comes in, allowing customers to easily scale their resources up or down based on demand.

Interestingly, only 19% of customers use autoscaling despite its benefits. For those who do use it, they are able to scale resources by two orders of magnitude, which is a significant increase. This feature allows customers to avoid overprovisioning their resources and only pay for what they need.

Readonly queries are also more common during office-hours on weekdays, which means that compute resources can be scaled down during non-peak times to save costs. Additionally, the cache hit ratios around 80%, indicating that frequently used data is being efficiently stored and accessed.

Overall, autoscaling is a valuable tool for Snowflake customers who have unpredictable workloads and need the flexibility to scale their compute resources on demand.

