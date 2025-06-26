# Classless Addressing

Introduced by [IETF](ietf/ietf) in 1993 to replace classful addressing. Its goal was to slow the growth of [routing tables](https://www.wikiwand.com/en/articles/Routing_table "Routing table") on [routers](https://www.wikiwand.com/en/articles/Router_\(computing\) "Router (computing)") across the Internet, and to help slow the rapid exhaustion of IPv4 addresses.

CIDR is based on **variable-length subnet masking** (**VLSM**), in which network prefixes have variable length (as opposed to the fixed-length prefixing of the previous classful network design). The main benefit of this is that it grants finer control of the sizes of subnets allocated to organizations, hence slowing the exhaustion of IPv4 addresses from allocating larger subnets than needed. CIDR gave rise to a new way of writing IP addresses known as CIDR notation, in which an IP address is followed by a suffix indicating the number of bits of the prefix. Some examples of CIDR notation are the addresses _192.0.2.0/24_ for IPv4 and _2001:db8::/32_ for IPv6.

## References

- https://www.wikiwand.com/en/articles/Classless_Inter-Domain_Routing
