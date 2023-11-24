# Syllabus
**Unit 1**
- Routing Algorithms
    - Introduction
    - global vs decentralized routing
    - The Link State(LS) Routing Algorithm
    - The Distance Vector (DV) Routing Algorithm
    - Hierarchical Routing
- Routing in the Internet
    - RIP
    - OSPF
    - BGP
- Introduction to Broadcast and Multicast Routing
## What is routing?
Network routing is the process of selecting a path across one or more networks.
- The principles of routing can apply to any type of network, from telephone networks to public transportation.
- These Internet routing decisions are made by specialized pieces of network hardware called [routers](https://www.cloudflare.com/learning/network-layer/what-is-a-router/).
Routers work in the following way: when a router receives a packet, it reads the headers* of the packet to see its intended destination, like the way a train conductor may check a passenger's tickets to determine which train they should go on. It then determines where to route the packet based on information in its routing tables.
- Routers do this millions of times a second with millions of packets. As a packet travels to its destination, it may be routed several times by different routers.
## Routing Algorithms
# References

> [!info] What is routing? | IP routing | Cloudflare  
> What is routing?  
> [https://www.cloudflare.com/learning/network-layer/what-is-routing/](https://www.cloudflare.com/learning/network-layer/what-is-routing/)