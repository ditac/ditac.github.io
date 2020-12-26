---
name: Amazon Elasticsearch Service
tools: [aws, elasticsearch, opendistro]
image: https://raw.githubusercontent.com/ditac/ditac.github.io/master/assets/aes.png
---

### Summary

I worked with a group of three other engineers to build Amazon Elasticsearch
Service. We were able to build a minimal implementation and onboarding private
beta customers within 6 months. Some of the intersting features that I would
like highlight were -
1. Online upgrades and the ability to move cluster between instance types.
2. Networking design and integration with the load-balancer.
3. Security.
4. Infrastructure provisioning - multi-az, multi-region, master/data nodes etc.

I will highlight some of the features and design choices that we made in each
of these areas.
