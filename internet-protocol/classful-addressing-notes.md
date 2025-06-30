# Classfull Addressing

- It is a 32 bit address.
- Represented in decimal notation in groups of 8 bits.
- Represented in binary notation in groups of 8 bits.
- Address are divided into 5 classes.
- Class A address
	- Starting bit `0` .
	- 8 network address bits.
	- 24 host address bits.
	- For general use.

- Class B addresses have starting bit `10`.
	- 12 network bits.
	- 12 host bits.
	- For general use.

- Class C addresses have starting bit `110`.
	- 24 nework bits.
	- 8 host bits.
	- For general use.

- Class D addresses have starting bit `1110`.
	- For multicasting.

- Class E addresses have starting bit `1111`.
	- For future use.

For class A, B and C, all host bits set to 0 is used to represent network address. And similarly all host bits set to 1 represent mulitcast address for that network.

---

- Introduced in 1981.
- Replaced by CIDR in 1993.
- Number of hosts is 2^n - 8