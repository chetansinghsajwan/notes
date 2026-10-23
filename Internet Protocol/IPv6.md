**IPv6 in short:** the successor to IPv4, built because IPv4 addresses ran out.
**Core points**
- Address size is 128 bits, against IPv4's 32 bits.
- Total addresses: about 3.4 × 10³⁸, effectively unlimited.
- Written in hexadecimal, 8 groups separated by colons, e.g. `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Shortening rules: drop leading zeros in each group and replace one run of zero groups with `::` (e.g. `2001:db8::1`).
- No NAT needed: every device can have its own public address.
- Autoconfiguration (SLAAC): devices can set up their own address without DHCP.
- Built-in IPsec support, and a simpler header for faster routing.
- No broadcast; it uses multicast and anycast instead.
**IPv4 vs IPv6**
|Feature|IPv4|IPv6|
|---|---|---|
|Address length|32 bits|128 bits|
|Format|Decimal, dots (192.168.1.1)|Hex, colons|
|Address space|~4.3 billion|~3.4 × 10³⁸|
|NAT|Widely needed|Not needed|
|Broadcast|Yes|No (multicast)|
|Config|Manual / DHCP|SLAAC / DHCPv6|
**Address types**
- **Unicast:** one-to-one communication.
- **Multicast:** one-to-many.
- **Anycast:** delivered to the nearest of several devices.