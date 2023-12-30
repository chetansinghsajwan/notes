# Unit 5

## Cache Coherence

Cache coherence refers to the consistency and synchronization of data stored in different caches within a multiprocessor system.

Cache coherence problem refers to the state when multiple processors hold different value of the same memory address in their caches.

### Cache Line State

- **Modified**
    
    A cache line in this state holds the most recent, correct copy of the data while the copy in the main memory is incorrect and no other processor holds a copy.

- **Owned**
    
    A cache line in this state holds the most recent, correct copy of the data. It is similar to shared state in that other processors can hold a copy of most recent, correct data and unlike shared state however, copy in main memory can be incorrect. Only one processor can hold the data in owned state while all other processors must hold the data in shared state.

- **Exclusive**
    
    A cache line in this state holds the most recent, correct copy of the data. The main memory copy is also most recent, correct copy of data while no other holds a copy of data.

- **Shared**
    
    A cache line in this state holds the most recent, correct copy of the data. Other processors in system may hold copies of data in shared state as well. The main memory copy is also the most recent, correct copy of the data, if no other processor holds it in owned state.

- **Invalid**
    
    A cache line in this state does not hold a valid copy of data. Valid copies of data can be either in main memory or another processor cache.

### Cache Coherence Protocols

These protocols aims to solve the cache coherence problem.

#### Snoopy Bus Protocols

Snoopy Bus Protocols rely on a shared bus that connects all caches in a multiprocessor system. These protocols monitor the bus for memory transactions and employ snooping logic to maintain cache coherence. Two popular policies used in Snoopy Bus Protocols are the Write-Invalidate policy and the Write-Update policy.

1. Write-Invalidate Policy: In the Write-Invalidate policy, when a cache performs a write operation on a shared memory block, it invalidates or marks as invalid all other copies of that block in other caches. When a cache wants to read a memory block, it first checks if any other cache holds a modified copy. If so, it requests the updated data from the cache that has the latest copy or from main memory.

2. Write-Update Policy: In the Write-Update policy, when a cache performs a write operation on a shared memory block, it updates the value in its own cache and also updates all other copies of that block in other caches. This approach reduces the need for subsequent cache-to-cache transfers for read operations. However, it requires more bus bandwidth for broadcasting updates to all caches.

Snooping Logic and Broadcast Messages: Snoopy Bus Protocols rely on snooping logic implemented in each cache controller. The snooping logic monitors bus transactions to determine if a cache’s copy of a memory block is still valid or needs to be invalidated or updated. When a cache performs a write operation, it broadcasts a message on the bus to inform other caches of the change. Caches with the snooping logic update their copies accordingly.

#### Directory-Based Protocols

Directory-Based Protocols use a centralized directory to maintain cache coherence information. The directory keeps track of which caches hold copies of each memory block and manages the permissions for accessing and modifying the blocks. Two key features of Directory-Based Protocols are a common directory for maintaining coherence and permission-based data sharing.

1. Common Directory for Maintaining Coherence: In Directory-Based Protocols, a central directory maintains the coherence information for all memory blocks. It keeps track of which caches have copies of each block and their respective states (e.g., shared, modified). Caches communicate with the directory to request permission for accessing and modifying memory blocks.

2. Permission-Based Data Sharing: Directory-Based Protocols manage data sharing through permissions. When a cache wants to read or write a memory block, it requests permission from the directory. The directory grants permission based on the current state of the block and the coherence protocol in use. Caches coordinate with the directory to ensure that they have the required permissions before performing operations on shared memory blocks.

Directory-Based Protocols generally have higher overhead due to the centralized directory. However, they can provide better scalability and reduce bus contention compared to Snoopy Bus Protocols in large-scale multiprocessor systems.
