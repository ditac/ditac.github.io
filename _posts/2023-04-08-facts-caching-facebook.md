---
title: Interesting facts about caching at Facebook
tags: [caching, Facebook, workload]
style: fill
color: primary
description: Interesting facts about caching at facebook. Mostly from the paper on the cachelib library.
---
Caching plays a crucial role in optimizing web performance and reducing server load. At Facebook, caching is an integral part of their infrastructure, serving 70% of web requests through CDN caches alone. In this blog post, we will explore some interesting facts about caching at Facebook.

Facebook has a diverse set of caching systems that cater to different use cases, including CDN caches, key-value application caches, socialgraph caches, and media caches. These caching systems were implemented independently in the past, which made it challenging to maintain and optimize them as their numbers grew.

To overcome these challenges, Facebook developed CacheLib, a C++ library that provides a common core of cache functionality. CacheLib has been in production since 2017 and powers over 70 different services at Facebook. It has facilitated an explosive growth in caches throughout Facebook since mid-2018 and has led to a significant reduction in backend load.

A single caching server at Facebook can replace tens of backend database servers by achieving 20× higher throughput and hit ratios exceeding 80%. This is because caching servers store frequently accessed data in memory, reducing the need to fetch data from disk or remote servers repeatedly.

CDN caches are another critical component of Facebook's caching infrastructure. They serve 70% of web requests at Facebook and reduce latency by an order of magnitude. CDN caches store static content like images and videos on edge servers located closer to users to improve performance.

In conclusion, caching plays an essential role in optimizing web performance and reducing server load at Facebook. With CacheLib's introduction as a common core for cache functionality, managing multiple independent cache systems became more manageable. The explosive growth in caches throughout Facebook since mid-2018 shows the impact CacheLib had on reducing backend load while improving overall system performance.

## References
[CacheLib](https://www.usenix.org/conference/osdi20/presentation/berg)
