# Classfull Addressing

- Introduced in 1981 in RFC 791 by IETF.
- Replaced by CIDR in 1993.
- Address are divided into 5 classes.

---
**Classes**

- Class A
	- Starting bit `0` .
	- Has 8 network address bits.
	- Has 24 host address bits.
	- For general use.
	- Starting address -> 0.0.0.0

- Class B
	- Starting bit `10`.
	- 12 network bits.
	- 12 host bits.
	- For general use.
	- Starting address -> 128.0.0.0

- Class C
	- Starting bit `110`.
	- 24 nework bits.
	- 8 host bits.
	- For general use.
	- Starting address -> 192.0.0.0

- Class D
	- Starting bit `1110`.
	- For multicasting.
	- Start address -> 224.0.0.0
	- End address -> 255.255.255.255

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

---
**Questions**

- How many number of hosts and networks in each class
- What are the starting and ending address of each class
- What are the decimal representations
