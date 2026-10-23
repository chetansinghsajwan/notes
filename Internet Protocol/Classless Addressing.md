CIDR stands for Classless Inter-Domain Routing.
- **What it is:** an addressing method with **no classes**. The network/host split can fall at any bit, written as a prefix length (e.g. `203.0.113.0/20`).
- **Introduced:** 1993 (RFC 1517-1519), later updated in RFC 4632 (2006).
- **Notation:** `address/prefix-length`, where the prefix is the number of network bits.
- **Why it was created:**
	- Classful sizes were too rigid, so addresses were wasted.
	- Routing tables were growing too fast.
**What CIDR changed**
|Classful|CIDR|
|---|---|
|Fixed splits (/8, /16, /24)|Any prefix from /0 to /32|
|First bits decide the class|First bits mean nothing|
|Need 300 hosts → Class B (65,534 usable)|Need 300 hosts → `/23` (510 usable)|
|One route per class network|Many networks combined into one route|
**Key ideas**
- **Variable-length prefix:** the allocation size is chosen to fit the need.
- **Route aggregation (supernetting):** several contiguous networks are advertised as one route, which keeps routing tables small.
- **Longest prefix match:** if several routes match a destination, the router picks the one with the longest prefix.
**Aggregation example**
- Four Class C networks: `192.168.0.0`, `192.168.1.0`, `192.168.2.0`, `192.168.3.0`
- Combined into one route: `**192.168.0.0/22**` (mask `255.255.252.0`)
- Check: 10 host bits = 1,024 addresses = 4 × 256
**Common prefixes**
|Prefix|Mask|Total addresses|Usable hosts|
|---|---|---|---|
|`/16`|`255.255.0.0`|65,536|65,534|
|`/20`|`255.255.240.0`|4,096|4,094|
|`/22`|`255.255.252.0`|1,024|1,022|
|`/23`|`255.255.254.0`|512|510|
|`/24`|`255.255.255.0`|256|254|
- **Subnetting vs CIDR:** subnetting splits a network into smaller ones (prefix gets longer), while CIDR also allows combining networks (prefix gets shorter).
- **India note:** Indian ISPs get CIDR blocks from the national registry **IRINN (run by NIXI)**, which is part of the APNIC region, and then assign smaller blocks to customers.
Next: **VLSM** (different-sized subnets inside one network), or a practice question?
# References

> [!info]  
>  
> https://www.wikiwand.com/en/articles/Classless_Inter-Domain_Routing