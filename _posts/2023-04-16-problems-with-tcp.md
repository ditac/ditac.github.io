---
title: Problems with tcp
tags: [latency, tcp, network, load-balancing]
style: fill
color: warning
description: Problems with tcp in the datacenter
---

1. Stream orientation is bad for software load balancing
2. In-order packet delivery restricts load balancing and causes hot spots
3. Long-lived connection state for each peer is undesirable in datacenter environments
4. Bandwidth sharing causes discrimination against short messages
5. Sender-driven congestion control has limitations as senders have no first-hand knowledge of congestion
6. Congestion control is limited by the fact that it can only detect congestion when there is buffer occupancy, leading to packet queueing and a trade-off between latency and throughput.
7.  TCP's reliability guarantees are not suitable for applications that require round trip guarantees because TCP only guarantees in-order delivery of packets, not timely delivery. This means that packets may be delayed or lost, which can lead to retransmission and increased latency. Applications that require round trip guarantees need to know that their messages will be delivered within a certain time frame to ensure timely responses. TCP's reliability guarantees do not provide this level of assurance.

TCP's problems impact systems at multiple levels because its design decisions, such as its stream orientation and requirement of in-order packet delivery, are not suitable for modern datacenters. These problems affect the network by causing congestion and limiting bandwidth sharing, the kernel software by adding complexity and overheads to load balancing, and applications by restricting message throughput and inducing head-of-line blocking. TCP's limitations also impact hardware load balancing, causing hot spots and increased latency. Therefore, a new protocol like Homa is needed to address these issues and provide a better solution for datacenter computing.

### Solution
Homa solves the problems with TCP by using a message-based protocol that implements remote procedure calls (RPCs) and is connectionless. It avoids TCP's stream orientation, which is disastrous for software load balancing, by using explicit message boundaries. Homa manages congestion from the receiver, not the sender, which is better because the receiver has knowledge of all incoming messages. Homa can tolerate out-of-order packet arrivals, which provides more flexibility for load balancing. Additionally, Homa uses priority queues to eliminate the tradeoff between latency and bandwidth and takes advantage of modern switches' priority queues to ensure that shorter messages are favored over longer ones. Finally, Homa provides end-to-end reliability for RPCs and implements mechanisms such as flow control, retry, and congestion control using per-RPC state.


## References
[Its time to replace TCP](https://arxiv.org/abs/2210.00714)
