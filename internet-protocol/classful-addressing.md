# Classful Addressing

It is network addressing architecture for the Internet introduced in 1981 in RFC 791 by IETF to divide address space into mulitiple categories. These categories are called classses.

It is obsoleted by the introduction of CIDR in 1993.

The method divides the IP address space for IPv4 into five address classes based on the leading four address bits.

These five classes are A, B, C, D and E.

---
**Class A**

- For general use.
- Starting bits: `0*` .
- Count of network address bits: 8
- Count of host address bits: 24
- Count of network addresses: 128 (`2^7`)
- Count of host addresses: 16,777,216 (`2^24`)
- First address: `0.0.0.0`
- Last address: `127.255.255.255`

---
**Class B**

- For general use.
- Starting bits: `10*`.
- Count of network address bits: 12
- Count of host address bits: 12
- Count of network addresses: 16,384 (`2^14`)
- Count of host addresses: 65,536 (`2^16`)
- First address: `128.0.0.0`
- Last address: `191.255.255.255`

---
**Class C**

- For general use.
- Starting bits: `110*`.
- Count of network address bits: 24
- Count of host address bits: 8
- Count of network addresses: 2,097,152 (2^21)
- Count of host addresses: 256 (`2^8`)
- First address: `192.0.0.0`
- Last address: `223.255.255.255`

---
**Class D**

- For multicasting.
- Starting bits: `1110*`.
- First address: `224.0.0.0`
- Last address: `239.255.255.255`

---
**Class E**

- For future use.
- Starting bits: `1111*`.
- First address: `240.0.0.0`
- Last address: `255.255.255.255`

---

For class A, B and C, all host bits set to `0` is used to represent network address.

And similarly all host bits set to `1` represent mulitcast address for that network.

---
**Number of hosts**

The number of hosts in a class can be calculated as `2^n - 2`, where n is number of host address bits. `- 2` is for network address and multicast address mentioned above.

---

**What was the idea behind classfull addressing?**

When classfull addressing was being designed, all network address were lower than 64 in decimal. So only the 7 least significant digits of the first octet were being used. Which means introducing a class with the first bit set to `0` would allow the old addresses to work perfectly fine.

---

**How was IPV4 before classful addressing?**

There were no classes. The first 8 bits were used for network address and the remaining bits were used for host address.

## References

- https://www.wikiwand.com/en/articles/Classful_network
