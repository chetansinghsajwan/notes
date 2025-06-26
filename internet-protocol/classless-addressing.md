# Classless Addressing

Introduced by [IETF](ietf/ietf) in 1993 to replace classful addressing. Its goal was to slow the growth of [routing tables](https://www.wikiwand.com/en/articles/Routing_table "Routing table") on [routers](https://www.wikiwand.com/en/articles/Router_\(computing\) "Router (computing)") across the Internet, and to help slow the rapid exhaustion of IPv4 addresses.

CIDR is based on **variable-length subnet masking** (**VLSM**), in which network prefixes have variable length (as opposed to the fixed-length prefixing of the previous classful network design). The main benefit of this is that it grants finer control of the sizes of subnets allocated to organizations, hence slowing the exhaustion of IPv4 addresses from allocating larger subnets than needed. CIDR gave rise to a new way of writing IP addresses known as CIDR notation, in which an IP address is followed by a suffix indicating the number of bits of the prefix. Some examples of CIDR notation are the addresses _192.0.2.0/24_ for IPv4 and _2001:db8::/32_ for IPv6.

**Subnet mask**

A subnet mask is a [bitmask](https://www.wikiwand.com/en/articles/Bitmask "Bitmask") that encodes the prefix length associated with an IPv4 address or network in quad-dotted notation: 32 bits, starting with a number of _1_-bits equal to the prefix length, ending with _0_-bits, and encoded in four-part dotted-decimal format: _255.255.255.0_. A subnet mask encodes the same information as a prefix length but predates the advent of CIDR. In CIDR notation, the prefix bits are always contiguous. Subnet masks were allowed by [RFC](https://www.wikiwand.com/en/articles/RFC_\(identifier\) "RFC (identifier)") [950](https://www.rfc-editor.org/rfc/rfc950)[[6]](https://www.wikiwand.com/en/articles/Classless_Inter-Domain_Routing#cite_note-RFC_950_2.1-6) to specify non-contiguous bits until [RFC](https://www.wikiwand.com/en/articles/RFC_\(identifier\) "RFC (identifier)") [4632](https://www.rfc-editor.org/rfc/rfc4632)[[5]](https://www.wikiwand.com/en/articles/Classless_Inter-Domain_Routing#cite_note-RFC_4632-5): Section 5.1  stated that the mask must be left contiguous. Given this constraint, a subnet mask and CIDR notation serve exactly the same function.

## References

- https://www.wikiwand.com/en/articles/Classless_Inter-Domain_Routing
