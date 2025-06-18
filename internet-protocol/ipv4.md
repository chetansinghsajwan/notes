# IPV4

An IPv4 address is a series of four eight-bit binary numbers separated by a decimal point. Although you may use any numbering system to represent a unique 32-bit number, most commonly you see IP addresses expressed in dot-decimal notation.

### Early IPv4 routing

Initially, the standard defined the first octet as a network identifier, but with only 256 unique values, the number of available networks quickly ran out. Several different changes made over the years have allowed for the extension of IPv4’s life. First came the division of the available addresses into five classes: A, B, C, D, and E.

The class system defined which class a network belongs in based on its first octet.

- Class A network’s first octet begins with 0. The first octet identifies the network. Class A supports 127 networks, each with 16 million hosts.

- Class B network’s first octet begins with 10. The first and second octets identify the network. Class B upports 16,000 networks, each with 65,000 hosts.

- Class C network’s first octet begins with 110. The first three octets identify the network. Class C supports 2 million networks, each with 254 hosts.

- Class D network’s first octet begins with 1110. Class D is reserved for multicast groups.

- Class E network’s first octet begins with 1111. Class E is reserved for future use.

Each class used a different number of bits to identify the network affecting how many networks and hosts each class could accommodate. For example, Class C’s first three octets described the network, while the fourth described the host on the network. Later the IETF replaced the class system, dubbed “classful,” with [subnet masks](https://en.wikipedia.org/wiki/IPv4#First_and_last_subnet_addresses/#First_and_last_subnet_addresses) that allowed for the dispersal of addresses on any address-bit boundary.

## IPv4 today

In 1993 the introduction of [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) (CIDR) gave greater flexibility for allocating blocks of addresses. CIDR adds a suffix to the IP address to identify how many of the leading bits represent the network address. For IPv4, that means a number between 0 and 32. The higher the suffix, the fewer available host addresses available on the network.

CIDR slowed the growth of routing tables and extended the life of IPv4 by reducing the number of wasted addresses that plagued the class system. CIDR is still the most widely used network routing method used today for both IPv4 and IPv6 routing.

## IPv4 address exhaustion

2011 saw the last distribution of IPv4 address blocks to the five regional Internet registries, one of which ran out of addresses completely within the next few months. The individual Internet service providers keep IPv4 alive by recycling addresses as they become available.

As noted earlier, IPv4 has a cap just short of 4.3 billion available addresses. With the Internet’s explosion in growth and the Internet of Things, the number of available addresses quickly depleted. To remedy the situation, the IETF released IPv6 with its 128-bit address space for a nearly inexhaustible 340 undecillions (340 followed by 37 zeros) available addresses. [Learn more about IPv4 address exhaustion](https://en.wikipedia.org/wiki/IPv4_address_exhaustion).

## IPv4 and IPv6 compatibility

Although IPv4 and IPv6 use CIDR to handle their network and host addressing, the two protocols are not interchangeable. IPv6 also fixes many other networking issues inherent in IPv4, such as smaller routing tables, simplified packet headers, and uses multicast instead of broadcast.

A single device can support both IPv4 and IPv6. [Dual-stack IP](https://en.wikipedia.org/wiki/IPv6#Dual-stack_IP_implementation) allows for a single router, switch, or server to process either address space. You cannot connect to an IPv6 only device using an IPv4 connection and vice-versa.

## References

- https://www.uptrends.com/what-is/ipv4
