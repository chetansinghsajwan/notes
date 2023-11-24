OSI Security Architecture focuses on these concepts:
- Security Attacks
- Security Services
- Security mechanism
## Security Attacks
A security attack is an attempt by a person or entity to gain unauthorized access to disrupt or compromise the security of a system, network, or device.
- These are defined as the actions that put at risk an organization’s safety.
They are further classified into 2 sub-categories:
1. **Passive Attack**
    
    Passive attacks are focused on gathering information or intelligence, rather than causing damage or disruption.
    
    - Attacker only reads the data.
    - There is no modification of data.
    - Hard to detect.
    - More emphasis is on prevention than detection.
    - Encryption prevents the success of passive attacks.
    
    Passive attacks are further divided into two parts based on their behavior:
    
    - **Eavesdropping** involves the attacker intercepting and listening to communications between two or more parties without their knowledge or consent.
        
        Eavesdropping can be performed using a variety of techniques, such as [**packet sniffing**](https://www.geeksforgeeks.org/what-is-packet-sniffing/), or man-in-the-middle attacks.
        
    - **Traffic analysis** involves the attacker analyzing network traffic patterns and metadata to gather information about the system, network, or device.
        
        Here the intruder can’t read the message but only understand the pattern and length of encryption.
        
        Traffic analysis can be performed using a variety of techniques, such as network flow analysis, or protocol analysis.
        
2. **Active Attacks**
    
    Active attacks refer to types of attacks that involve the attacker actively disrupting or altering system, network, or device activity.
    
    Active attacks are typically focused on causing damage or disruption, rather than gathering information or intelligence.
    
    - There is modification of data.
    - Hard to prevent.
    - Authentication and authorization are used to prevent this.
    - Data backup and recovery is used to resolve this.
    
    Active attacks are further divided into four parts based on their behavior:
    
    - **Masquerade** is a type of attack in which the attacker pretends to be an authentic sender in order to gain unauthorized access to a system. This type of attack can involve the attacker using stolen or forged credentials.
    - **Replay** is a type of active attack in which the attacker intercepts a transmitted message through a passive channel and then maliciously or fraudulently replays or delays it at a later time.
    - **Modification of Message** involves the attacker modifying the transmitted message and making the final message received by the receiver. This type of attack can be used to manipulate the content of the message or to disrupt the communication process.
    - **Denial of service (DoS)** attacks involve the attacker sending a large volume of traffic to a system, network, or device in an attempt to overwhelm it and make it unavailable to legitimate users.
## Security Services
Security services refer to the different services available for maintaining the security and safety of an organization.
Security services are divided into 5 types:
- **Authentication** is the process of verifying the identity of a user or device in order to grant or deny access to a system or device.
- **Access control** determines who is allowed to access specific resources within a system.
- **Data Confidentiality** is responsible for the protection of information from being accessed or disclosed to unauthorized parties.
- **Data integrity** is a security mechanism that involves the use of techniques to ensure that data has not been tampered with or altered in any way during transmission or storage.
- **Non-repudiation** involves the use of techniques to create a verifiable record of the origin and transmission of a message, which can be used to prevent the sender from denying that they sent the message.
## Security Mechanisms
Security mechanisms are the specific techniques, protocols, or tools implemented to achieve security services.
They are the practical implementations that enforce security policies and ensure the protection of data and resources.
Some examples of security mechanisms include:
- **Encryption** involves the use of algorithms to transform data into a form that can only be read by someone with the appropriate decryption key or knowledge.
- **Digital signature** is a security mechanism that involves the use of cryptographic techniques to create a unique, verifiable identifier for a digital document or message, which can be used to ensure the authenticity and integrity of the document or message.
- **Traffic padding** is a technique used to add extra data to a network traffic stream in an attempt to obscure the true content of the traffic and make it more difficult to analyze.
- **Routing control** allows the selection of specific physically secure routes for specific data transmission and enables routing changes, particularly when a gap in security is suspected.
# References

> [!info] OSI Security Architecture - GeeksforGeeks  
> A Computer Science portal for geeks.  
> [https://www.geeksforgeeks.org/osi-security-architecture/](https://www.geeksforgeeks.org/osi-security-architecture/)  

> [!info] Types of Security Mechanism - GeeksforGeeks  
> A Computer Science portal for geeks.  
> [https://www.geeksforgeeks.org/types-of-security-mechanism/](https://www.geeksforgeeks.org/types-of-security-mechanism/)