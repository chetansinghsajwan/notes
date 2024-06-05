# Unit 3

## Data management

### Issues

- **Limited storage capacity**: Mobile devices typically have limited storage capacity, which can make it difficult to store and manage large amounts of data. This can require careful consideration of data storage and compression techniques.

- **Network connectivity**: Mobile devices are often connected to networks with limited bandwidth or intermittent connectivity, which can make it difficult to manage data in real-time. This can require careful consideration of data synchronization and caching techniques.

- **Security**: Mobile devices are often used to access sensitive or confidential data, which can make them vulnerable to security breaches. This can require careful consideration of data encryption and authentication techniques.

- **Data fragmentation:** Mobile devices are often used to access data from multiple sources, which can result in data fragmentation and duplication. This can require careful consideration of data consolidation and integration techniques.

- **Data inconsistency:** Mobile devices are often used in environments where data can change rapidly or be updated frequently, which can result in data inconsistency. This can require careful consideration of data versioning and conflict resolution techniques.

- **Data privacy:** Mobile devices are often used to collect and store personal data, which can raise privacy concerns. This can require careful consideration of data anonymization and data privacy regulations.

### Advantages

- **Portability:** Mobile databases can be accessed from anywhere, making it possible to access and update data on the go.

- **Convenience:** Mobile databases are convenient and easy to use, allowing users to quickly access and update data from their mobile devices.

- **Real-time updates:** Mobile databases can be synchronized in real-time, allowing users to access the most up-to-date data.

- **Improved productivity:** Mobile databases can improve productivity by allowing users to access and update data quickly and easily from their mobile devices.

- **Reduced costs:** Mobile databases can reduce costs by eliminating the need for paper-based record keeping and reducing the need for physical storage space.

### Disadvantages

- **Limited storage capacity:** Mobile devices have limited storage capacity compared to desktop computers or servers, which can limit the amount of data that can be stored on a mobile device.

- **Connectivity issues:** Mobile devices may have limited or intermittent connectivity to networks, which can affect the ability to access or synchronize data.

- **Security concerns:** Mobile devices may be more vulnerable to security breaches, such as data theft or hacking, due to their portability and the potential for loss or theft.

- **Data synchronization issues:** Mobile devices may not always be able to synchronize data with other devices or servers in real-time, which can result in data discrepancies or conflicts.

- **Platform-specific issues:** Mobile databases may be designed to run on specific operating systems, which can limit their portability or interoperability with other systems.

## Data replication

It is the process of storing data in more than one site or node. It is useful in improving the availability of data. It is simply copying data from a database from one server to another server so that all the users can share the same data without any inconsistency. The result is a ****distributed database**** in which users can access data relevant to their tasks without interfering with the work of others.

### Replication Schemas

There are three types of replication schemas:

- **Full Replication:** The most extreme case is replication of the whole database at every site in the distributed system. This will improve the availability of the system because the system can continue to operate as long as atleast one site is up.

- **Partial Replication**: Partial replication means, some fragments are replicated whereas others are not. Only a subset of the database is replicated at each site. This reduces storage costs but requires careful planning to ensure data consistency.

- **No Replication:** No replication means, each fragment is stored exactly at one site.

### Advantages

**Increased Availability:** Data replication can improve availability by providing multiple copies of the same data in different locations, which reduces the risk of data unavailability due to network or hardware failures.

**Improved Performance:** Replicated data can be accessed more quickly since it is available in multiple locations, which can help to reduce network latency and improve query performance.

**Enhanced Scalability:** Replication can improve scalability by distributing data across multiple nodes, which allows for increased processing power and improved performance.

**Improved Fault Tolerance:** By storing data redundantly in multiple locations, replication can improve fault tolerance by ensuring that data remains available even if a node or network fails.

**Improved Data Locality:** Replication can improve data locality by storing data close to the applications or users that need it, which can help to reduce network traffic and improve performance.

**Simplified Backup and Recovery:**** Replication can simplify backup and recovery processes by providing multiple copies of the same data in different locations, which reduces the risk of data loss due to hardware or software failures.

**Enhanced Disaster Recovery:** Replication can improve disaster recovery capabilities by providing redundant copies of data in different geographic locations, which reduces the risk of data loss due to natural disasters or other events.

### Disadvantages

- Increased complexity, as the replication process needs to be configured and maintained.

- Increased risk of data inconsistencies, as data can be updated simultaneously on different replicas.

- Increased storage and network usage, as multiple copies of the data need to be stored and transmitted.

- Data replication is widely used in various types of systems, such as online transaction processing systems, data warehousing systems, and distributed systems.

## CODA File system

CoDA stands for Context Delivery Architecture.

The Coda File System is a distributed file system. It aims to provide high availability and performance for networked file systems, especially in the presence of network and server failures. Coda is known for its strong support of disconnected operations, making it suitable for mobile computing environments.

#### Features of Coda File System

1. **Disconnected Operation:**
    
    - **Caching and Hoarding:** Coda allows users to continue working even when the network connection to the server is lost. It caches files locally and prefetches (hoards) files based on usage patterns and explicit user preferences.
    - **Reintegration:** When the network connection is re-established, Coda reintegrates the changes made during the disconnection back to the server.

2. **Replication:**
    
    - Coda supports server replication, meaning that files can be stored on multiple servers to improve availability and reliability.

3. **Server Volumes:**
    
    - Files are organized into volumes, which can be replicated across multiple servers.

4. **Client-side Caching:**
    
    - Coda clients maintain a cache of files that can be used even when disconnected from the server.

5. **Version Conflict Resolution:**
    
    - Coda has mechanisms for detecting and resolving conflicts that arise when multiple clients modify the same file during disconnection.

6. **Scalability:**
    
    - The system is designed to scale to a large number of clients and servers.

7. **Security:**
    
    - Coda uses Kerberos for authentication and supports access control lists (ACLs) for fine-grained access control.

#### Advantages of Coda File System

1. **High Availability:**
    
    - The ability to operate in disconnected mode and the use of replication ensures high availability of data, even in the face of network or server failures.

2. **Performance:**
    
    - Local caching and hoarding improve performance by reducing the need to access remote servers frequently.

3. **Flexibility:**
    
    - Coda’s support for disconnected operations makes it particularly suitable for mobile computing, where network connectivity can be intermittent.

4. **Fault Tolerance:**
    
    - Server replication and client caching provide robustness against server failures and network issues.

5. **Transparency:**
    
    - Users experience a seamless environment whether they are connected to the network or working offline.

#### Disadvantages of Coda File System

1. **Complexity:**
    
    - The mechanisms for caching, hoarding, and reintegration add complexity to the system, making it more challenging to administer and maintain.

2. **Conflict Resolution:**
    
    - Despite its mechanisms for conflict resolution, some conflicts may be difficult to resolve automatically, requiring user intervention.

3. **Scalability Issues:**
    
    - While designed to be scalable, managing a large number of replicas and clients can introduce performance and consistency challenges.

4. **Resource Intensive:**
    
    - Local caching and replication can consume significant storage and processing resources on clients and servers.

5. **Network Dependency:**
    
    - Though designed to handle disconnections, the system performs best with reliable network connectivity. Frequent or prolonged disconnections can complicate synchronization and conflict resolution.

## Disconnected operations

Disconnected operations in mobile computing refer to the capability of a system to continue functioning even when it is not connected to the central network or server. This is particularly crucial in mobile environments, where network connectivity can be intermittent or unreliable. Disconnected operations ensure that users can still access and modify data during periods of disconnection and that these changes are synchronized once connectivity is restored.

#### Key Concepts of Disconnected Operations

1. **Caching:**
    
    - Storing frequently accessed data locally on the client device to ensure it is available during disconnection.

2. **Hoarding:**
    
    - Pre-fetching and storing data that is likely to be needed during disconnection based on user preferences and past usage patterns.

3. **Logging:**
    
    - Keeping a record of changes made during the disconnected state to facilitate synchronization once the connection is re-established.

4. **Reintegration:**
    
    - The process of merging changes made during disconnection back to the central server when the connection is restored.

5. **Conflict Detection and Resolution:**
    
    - Mechanisms to detect conflicts that arise when multiple clients modify the same data while disconnected and strategies to resolve these conflicts.

#### Importance of Disconnected Operations

1. **Reliability:**
    
    - Ensures continued access to critical data and applications regardless of network connectivity.

2. **Productivity:**
    
    - Users can continue their work without interruption, increasing overall productivity.

3. **Flexibility:**
    
    - Supports mobile and remote work by allowing users to operate in diverse environments with varying connectivity.

4. **User Experience:**
    
    - Provides a seamless user experience by minimizing disruptions due to network issues.

#### Strategies for Implementing Disconnected Operations

1. **Efficient Caching:**
    
    - Implement robust caching mechanisms to store essential data locally. This may include read-only caches for frequently accessed data and read-write caches for data that needs to be modified.

2. **Intelligent Hoarding:**
    
    - Use algorithms to predict which data will be needed and prefetch it to the local cache. User preferences and historical data access patterns can guide this process.

3. **Synchronization Protocols:**
    
    - Develop efficient protocols for synchronizing changes made during disconnection with the central server. This includes detecting changes, merging them, and resolving conflicts.

4. **Conflict Resolution Policies:**
    
    - Establish clear policies for resolving conflicts, such as last-writer-wins, user intervention, or automated merge strategies.

5. **User Notifications:**
    
    - Inform users about the state of the network connection, the availability of data, and any conflicts that need to be resolved.

#### Examples of Systems Supporting Disconnected Operations

1. **Coda File System:**
    
    - Coda is designed to support disconnected operations through local caching, hoarding, and reintegration mechanisms. It uses server replication and client-side caching to ensure data availability and consistency.

2. **Microsoft OneDrive:**
    
    - OneDrive allows users to mark files as available offline, enabling them to access and modify these files without an active internet connection. Changes are synchronized once the connection is restored.

3. **Google Drive:**
    
    - Google Drive provides offline access to files through its offline mode, which caches selected files locally and synchronizes changes when the device reconnects to the internet.

4. **Lotus Notes:**
    
    - Lotus Notes supports disconnected operations by allowing users to work with local replicas of databases, which are synchronized with the server when connectivity is restored.

#### Challenges of Disconnected Operations

1. **Data Consistency:**
    
    - Ensuring data consistency across multiple clients and the central server can be challenging, especially when conflicts arise.

2. **Resource Management:**
    
    - Managing the local storage and processing resources required for caching and hoarding can be demanding, particularly on devices with limited resources.

3. **Conflict Resolution:**
    
    - Automatically resolving conflicts without user intervention is complex and may not always be possible.

4. **Security:**
    
    - Ensuring data security and integrity while storing sensitive information locally and during synchronization.
