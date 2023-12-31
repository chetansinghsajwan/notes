# Attribute Selection Operator

**name**:: Attribute Selection
**syntax**:: _attrset_ `.` _attrpath_ [ `or` _expr_ ]
**precedence**:: 1
**associativity**:: none

Select the attribute denoted by attribute path _attrpath_ from attribute set `attrset`.

If the attribute doesn’t exist, return the _expr_ after `or` if provided, otherwise abort evaluation.

An attribute path is a dot-separated list of [attribute names](https://nixos.org/manual/nix/stable/language/values#attribute-set).

> **Syntax**
> 
> _attrpath_ = _name_ [ `.` _name_ ]...