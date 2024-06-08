# Unit 4

## Mobile Agents

Mobile Agents is an autonomous program that can autonomously move from one host to another host and continue its execution on the destination host.

In this process, the chance of data loss is ver low because the state of the running program is saved and then transported to the new host.

This allows the program to continue execution from where it left off before migration.

Mobile Agents are also called as transportable agents.

They are classified into two types:

- **Mobile Agents with pre-defined path:** They have a static migration path.

- **Mobile Agents with undefined path:** They have dynamic migration paths. The mobile agents choose their path according to the present network condition.
  
  They are also known as **Roamer**.

### Features

- **Intelligence:** Mobile Agents are capable of learning and searching for knowledge about their domain. That's why they are called intelligent agents because they possess a degree of domain knowledge. They can also transport their state from one environment to another without disturbing the previous holding data

- **Autonomous:** They can take decisions without automatically based on their internal state, which means they do no need constant human interaction.

- **Mobility:** They can migrate from one node to another and can carry out tasks along with them. This feature distributes the processing and balancing of the load.

- **Communication:** Mobile Agents can communicate effectively with other agents, users and systems. The mobile agents use a communication language for inter-agent communication.

### Advantages

- The most significant advantage of mobile agents is the possibility of moving complex processing functions to the location where you have enormous amounts of data and that have to be processed.
- They are autonomous and self-driven in nature.
- They are maintenance-friendly or easily maintainable.
- They are Fault-tolerant. It means they are able to operate without an active connection between client and server.
- They reduce the compilation time.
- They provide less delay in the network.
- They provide fewer loads on the network.
- They facilitate parallel processing. It means they can be asynchronously executed on multiple heterogeneous network hosts.
- They provide dynamic adaptation in which their actions are dependent on the state of the host environment.

### Disadvantages

- The most significant disadvantage of mobile agents is their security. They are less secured

### Applications

- E-commerce, traffic control, network management, robotics, data-intensive applications, grid computing, parallel computing, distributed computing and mobile computing etc.

## Security and Fault Tolerance

Fault Tolerance is the ability of a system to continue operating despite failures in the system.

## Transaction Processing in Mobile Computing Environment

A transaction processing system allows application programmers to concentrate on writing code that will allow users to perform transactions simultaneously without bothering about what other users may be doing with their transactions at the same time:

- It manages the concurrent processing of transactions. 
- It enables the sharing of data. 
- It ensures the integrity of data.

Database applications are normally structured into transactions. The transaction is a type of operation that makes sure that database does not change into an inconsistent state to disrupt the transactions.

### **Issues in Transaction Processing**

One important aim in the design of any database system is to maximize the number of transactions that can be active at a time. DBMS ensures serializability using ACID constraints:

- Atomicity
- Consistency
- Isolation
- Durability

ACID properties have been redefined to support transactions in the mobile environment are: 

- **Atomicity Relaxation:** Mobile Host is allowed to submit pieces of the transaction from different cells according to the movement. It requires the ability to break a transaction into many sub-transactions that can be concurrently executed. 

- **Consistency Relaxation:** The database is logically partitioned into “clusters” based on some attributes. Data in the same cluster must be strictly consistent. Although the bounded degree of inconsistency is tolerated among the clusters.   

- **Isolation Relaxation:** The intermediate results of a transaction can be observed by other concurrent transactions. For example, if T1 is a transaction process and T2 is another transaction process then T1 should not be visible to T2. 

- **Durability Relaxation:** A disconnected Mobile Host can only commit a transaction locally if this transaction does not conflict with other transactions executed on the same HOST while HOST was disconnected.

### Transaction Processing Environment

There are 4 types of transaction processing environments:

1. **Centralized Environment:** Single user system executes all the transactions. 

2. **Client Server Environment:** Transaction and transaction initiation are done by the server and client respectively. Many clients can send transactions to servers simultaneously. 

3. **Distributed Environment:** Data is distributed over a network. The transaction can occur fully on a node or partially on a different node. 

4. **Mobile Environment:** Special type of distributed environment can accommodate user movements while processing transactions.

### **Issues in Mobile Environment**

- **User Movement:** Tracking users, and data recovery are complicated. LOG location determination is complex. 

- **Disconnections:** There may be temporary disconnections due to noise, fading of signal, handoff, etc.

- **Poor Communication Media-** Bandwidth allocated to mobile users could be very low. Interference from other traffic, noise, etc may corrupt data.

- **Processing Power-** With a less powerful CPU, database server operation is difficult.

- **Memory-** Memory availability is limited.

- **Battery Power-** Like memory, battery power is also limited.

- **User Interface-** It should be designed keeping in mind resource restrictions. 

- **Security-** Chances of data theft and unauthorized access increases while MH moves from one cell to another.
