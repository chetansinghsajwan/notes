# Platform Initialization Specification

This specification defines the boot process of a uefi based system.

The boot process is dividied into six stages:

1. SEC (Security Phase)
2. PEI (Pre EFI Intialization)
3. DXE (Driver Execution Envrionment)
4. BDS (Boot Device Select)
5. TSL (Transient System Load)
6. RT (Runtime)

## SEC

It stands for Security Phase.

It is reponsible for:
- Handling all platform restart events
- Creating a temporary memory store
- Serving as the root of trust in the system
- Passing handoff information to the PEI Foundation

### Handling all platform restart events



## References

- https://www.wikiwand.com/en/articles/UEFI#Boot_stages
- https://uefi.org/specs/PI/1.8/V1_Security_SEC_Phase_Information.html
