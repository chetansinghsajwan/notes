## Overview
- Popularized after DNS.
- Network addressing architecture for the Internet, introduced in 1981 in RFC 791
- Divides the IPv4 address space into **five classes** (A, B, C, D, E) based on the leading address bits
- Made obsolete by **CIDR** in 1993
## Class Summary
|Class|Purpose|Starting Bits|Network Bits|Host Bits|Networks|Addresses per Network|First Address|Last Address|
|---|---|---|---|---|---|---|---|---|
|A|General use|`0`|8|24|128 (2⁷)|16,777,216 (2²⁴)|`0.0.0.0`|`127.255.255.255`|
|B|General use|`10`|16|16|16,384 (2¹⁴)|65,536 (2¹⁶)|`128.0.0.0`|`191.255.255.255`|
|C|General use|`110`|24|8|2,097,152 (2²¹)|256 (2⁸)|`192.0.0.0`|`223.255.255.255`|
|D|Multicasting|`1110`|n/a|n/a|n/a|n/a|`224.0.0.0`|`239.255.255.255`|
|E|Future use|`1111`|n/a|n/a|n/a|n/a|`240.0.0.0`|`255.255.255.255`|
## Special Addresses (Classes A, B, C)
- All host bits `0` → **network address**
- All host bits `1` → **broadcast address** for that network
## Number of Usable Hosts
- Formula: `2ⁿ - 2`, where n = number of host bits
- The `2` accounts for the network and broadcast addresses
## Idea Behind Classful Addressing
- All existing network addresses were below 64 (decimal)
- So only the 7 least significant bits of the first octet were in use
- Adding a class with first bit `0` let the old addresses keep working unchanged
## IPv4 Before Classful Addressing
- No classes
- First 8 bits = network address
- Remaining bits = host address
## Timeline
|Year|Event|Owner|
|---|---|---|
|1974|TCP/IP design paper published|Vint Cerf (Stanford) and Bob Kahn (DARPA)|
|1981|RFC 791 defines IPv4 and classful addressing (Classes A-E)|Jon Postel, USC Information Sciences Institute (funded by DARPA)|
|1983|ARPANET switches fully to TCP/IP (1 January)|DARPA / ARPANET community|
|1985|RFC 950 adds subnetting|Jeffrey Mogul (Stanford) and Jon Postel|
|1986|IETF is founded|IETF|
|1986|ERNET, India's first academic network, starts|Dept. of Electronics, Govt. of India (with UNDP support)|
==What is the need of categorization like classful addressing?==
**Different network sizes:** a university, a telecom and a small office need very different numbers of addresses. One fixed split would either waste addresses or fall short, so three sizes (A, B, C) were created.

**Why did classful addressing fail?**
The sizes were too rigid. An organisation needing 300 hosts got a Class B (65,536), wasting thousands of addresses. This led to subnetting (1985) and finally CIDR (1993).
Find subnetting [[Subnetting]].
