- **What it is:** splitting one IP network into smaller networks (subnets) by borrowing bits from the host part and using them as extra network bits.
- **Introduced:** RFC 950 (1985), as the first fix for the rigid sizes of classful addressing.
- **Subnet mask:** a 32-bit value that marks which bits are network (1s) and which are host (0s). Example: `255.255.255.0` = `/24`.
- **Core idea:** the more bits you borrow, the more subnets you get, but the fewer hosts per subnet.
**Why it's needed**
- **Less waste:** a Class B (65,536 addresses) can be cut into smaller pieces that fit actual needs.
- **Smaller broadcast domains:** broadcasts stay inside each subnet, so less network noise.
- **Better organisation:** separate subnets for departments (e.g. Admin, Labs, Wi-Fi).
- **Security and control:** apply firewall rules between subnets.
**Formulas**
- Subnets = 2^(borrowed bits)
- Usable hosts per subnet = 2^(host bits) - 2
**Example: splitting** `**192.168.1.0/24**` **into 4 subnets (borrow 2 bits, new mask** `**/26**`**)**
|Subnet|Network address|Usable range|Broadcast address|
|---|---|---|---|
|1|`192.168.1.0/26`|`.1` to `.62`|`192.168.1.63`|
|2|`192.168.1.64/26`|`.65` to `.126`|`192.168.1.127`|
|3|`192.168.1.128/26`|`.129` to `.190`|`192.168.1.191`|
|4|`192.168.1.192/26`|`.193` to `.254`|`192.168.1.255`|
- **Each subnet:** 6 host bits, so 2⁶ - 2 = **62** usable hosts.
**Common masks**
|Prefix|Subnet mask|Usable hosts|
|---|---|---|
|`/24`|`255.255.255.0`|254|
|`/25`|`255.255.255.128`|126|
|`/26`|`255.255.255.192`|62|
|`/27`|`255.255.255.224`|30|
|`/28`|`255.255.255.240`|14|
|`/30`|`255.255.255.252`|2|
- **Subnetting vs CIDR:** subnetting divides a network _inside_ an organisation, while CIDR (1993) removed class boundaries across the whole Internet. Both use the same prefix-length idea.
==How to calculate the masing?==
- **Mask = prefix length written as bits:** `/26` means the first **26 bits are 1s** and the remaining 6 are 0s.
- **Step 1:** write 26 ones, then 6 zeros (32 bits total).
- **Step 2:** group into 4 octets of 8 bits.
- **Step 3:** convert each octet to decimal.
**Example:** `**/26**`
|Octet 1|Octet 2|Octet 3|Octet 4|
|---|---|---|---|
|`11111111`|`11111111`|`11111111`|`11000000`|
|255|255|255|**192**|
- **Why 192:** the last octet is `11000000`, so add the place values of the 1s: 128 + 64 = **192**.
**Bit place values (per octet)**
|Bit position|1st|2nd|3rd|4th|5th|6th|7th|8th|
|---|---|---|---|---|---|---|---|---|
|Value|128|64|32|16|8|4|2|1|
**Last octet value by number of 1s (the shortcut)**
|Prefix|1s in last octet|Calculation|Last octet|
|---|---|---|---|
|`/24`|0|0|0|
|`/25`|1|128|128|
|`/26`|2|128+64|192|
|`/27`|3|128+64+32|224|
|`/28`|4|128+64+32+16|240|
|`/29`|5|...+8|248|
|`/30`|6|...+4|252|
- **Quick trick:** last octet = 256 - (block size), where block size = 2^(host bits). For `/26`: 2⁶ = 64, so 256 - 64 = **192**.
- **Same trick gives the subnet jumps:** subnets start every 64 addresses (`.0`, `.64`, `.128`, `.192`), which is what the earlier table showed.
  
How are subnets passed alongside the address?
- **Not in the packet:** the IPv4 header carries only the source and destination addresses. The subnet mask is never sent with each packet.
- **Stored on each device:** every host and router keeps its own IP address and mask in its configuration.
- **Written together as:** `192.168.1.10/26` (CIDR notation) or `192.168.1.10` + `255.255.255.192` (address + mask).
**How devices get the mask**
|Method|How it works|
|---|---|
|DHCP|Server gives IP, mask (option 1), gateway and DNS together. This is the most common method.|
|ICMP Address Mask Request/Reply|Defined in RFC 950 to ask for the mask. Now obsolete.|
|Manual (static) config|Admin types in the IP and mask.|
|Routing protocols (OSPF, BGP, RIPv2, EIGRP)|Routers advertise each route as network + prefix length (e.g. `10.0.0.0/8`).|
- **RIPv1 didn't send masks:** it was classful, so it assumed the mask from the class. This is why it could not support subnetting properly.
**How the mask is used (local vs remote check)**
- The device does a bitwise AND of an address with its mask to get the network part.
- Example: you are `192.168.1.10/26` and the destination is `192.168.1.70`.
|Address|AND with `255.255.255.192`|Network|
|---|---|---|
|`192.168.1.10` (you)|last octet 10 → 0|`192.168.1.0`|
|`192.168.1.70` (destination)|last octet 70 → 64|`192.168.1.64`|
- **Result:** the networks differ, so the packet goes to the gateway.
- **Routers do the same:** they match the destination against each route's prefix in the routing table and pick the longest match.
==So subnetting completely overrides classful addressing, and not as an extension?==
- **Subnetting was an extension, not an override.** It was added in 1985 (RFC 950) on top of classful addressing and worked _with_ it.
- **Classes still decided the base network.** A Class B address still had a fixed 16-bit network part, and subnetting only split the _host_ part further.
- **It added a third level:** network / subnet / host (instead of just network / host).
- **Subnets were invisible outside the organisation.** The wider Internet still routed by the classful network, and only internal routers used the subnet mask.
- **CIDR (1993) is what replaced classful addressing.** It removed classes entirely, so the prefix length can be anything (e.g. `/20`) and the first bits no longer decide the split.
- **Subnetting itself survived.** It is still used today, now with CIDR prefix lengths.
|Year|Mechanism|Relation to classful addressing|
|---|---|---|
|1981|Classful addressing|Original scheme|
|1985|Subnetting|Extends it by splitting the host bits|
|1993|CIDR|Replaces it and generalises subnetting|