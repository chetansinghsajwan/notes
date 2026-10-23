**Basics**
- IPv4 address = 32-bit number, written as four 8-bit numbers (called octets) separated by periods, e.g. `192.0.2.1`
- Each octet ranges from 0 to 255 (2^8)
- Total: ~4.3 billion addresses (2³²), but many are reserved and can't be assigned to ordinary hosts
- Historical addressing system: see the separate [[Classful Addressing]] note
**CIDR (1993 onwards)**
- Adds a suffix (`/0` to `/32`) showing how many leading bits form the network prefix
- Larger prefix = fewer host bits = fewer addresses in that network
- Made allocation more efficient and slowed global routing-table growth
- Still widely used for both IPv4 and IPv6
**Address exhaustion**
- Causes: internet growth, mobile connectivity, cloud computing, IoT
- Feb 2011: IANA allocated its last IPv4 blocks to the five Regional Internet Registries (RIRs)
- RIRs ran out of their pools at different times afterwards
- IPv4 is kept alive by:
	- Address recycling
	- **NAT** (many devices share one public IP)
	- More efficient allocation
- **IPv6** is the long-term fix: 128-bit space, ~3.4 × 10³⁸ addresses, simpler packet headers, more scalable network design
**IPv4 vs IPv6 compatibility**
- Both use prefix-based addressing, but they are separate protocols and not directly interchangeable
- An IPv4-only device can't talk to an IPv6-only device without **translation or tunneling**
- **Dual-stack** lets routers, switches and servers handle both at once
- Transition is gradual; many networks run both in parallel
**India-specific**
- Indian IPv4 blocks come from **APNIC**, which exhausted its main pool in 2011
- Many Indian ISPs use **CGNAT** to stretch scarce IPv4 addresses
- **Jio** is IPv6-first, so India has among the highest IPv6 adoption rates
- **NIXI** runs IRINN, India's IP address registry
**Reference**
- https://www.uptrends.com/what-is/ipv4