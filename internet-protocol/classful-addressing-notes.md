# Classful Addressing

- Introduced in 1981 in RFC 791 by IETF.
- Replaced by CIDR in 1993.
- Address are divided into 5 classes.

---
**Classes**

- Class A
	- For general use.
	- Starting bit `0` .
	- Has 8 network address bits.
	- Has 24 host address bits.
	- Has 128 (`2^7`) networks.
	- Has 16,777,216 (`2^24`) hosts.
	- Starting address -> 0.0.0.0
	- End address -> 127.255.255.255

- Class B
	- For general use.
	- Starting bit `10`.
	- 12 network bits.
	- 12 host bits.
	- Has 16,384 (`2^14`) networks.
	- Has 65,536 (`2^16`) hosts.
	- Starting address -> 128.0.0.0
	- End address -> 191.255.255.255

- Class C
	- For general use.
	- Starting bit `110`.
	- 24 nework bits.
	- 8 host bits.
	- Has 2,097,152 (2^21) networks.
	- Has 256 (`2^8`) hosts.
	- Start address -> 192.0.0.0
	- End address -> 223.255.255.255

- Class D
	- Starting bit `1110`.
	- For multicasting.
	- Start address -> 224.0.0.0
	- End address -> 239.255.255.255

- Class E
	- Starting bit `1111`.
	- For future use.
	- Start address -> 240.0.0.0
	- End address -> 255.255.255.255

For class A, B and C, all host bits set to `0` is used to represent network address.

And similarly all host bits set to `1` represent mulitcast address for that network.

---
**Number of hosts**

The number of hosts in a class can be calculated as `2^n - 2`, where n is number of host address bits. `- 2` is for network address and multicast address mentioned above.

---

**What was the idea behind classfull addressing?**

When classfull addressing was being designed, all network address were lower than 64 in decimal. So only the 7 least significant digits were being used of the first octet. Which means introducing a class with the first bit set to `0` would allow the old addresses to work perfectly fine.

---

**How was IPV4 before classful addressing?**

There were no classes. The first 8 bits were used for network address and the remaining bits were used for host address.

## References

- https://www.wikiwand.com/en/articles/Classful_network
