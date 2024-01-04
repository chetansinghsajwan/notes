# Unit 1

## OSI Security Architecture

The OSI (Open Systems Interconnection) Security Architecture defines a systematic approach to providing security at each layer.

OSI security architecture is divided into three layers:

1. Security Attacks
2. Security Mechanisms
3. Security Services

### Security Attacks

A security attack is an attempt by a person or entity to gain unauthorized access to disrupt or compromise the security of a system, network, or device.

These are defined as the actions that put at risk an organization’s safety. They are further classified into 2 sub-categories:

1. Passive Attacks
2. Active Attacks

#### Passive Attacks

Attacks in which a third-party intruder tries to access the message or content of the data being shared.

These types of attacks involve the attacker observing or monitoring system, network, or device activity without actively disrupting or altering it.

These are divided into two types based on their behaviour:

1. Eavesdropping
2. Traffic Analysis

##### Eavesdropping

This involves the attacker intercepting and listening to communications between two or more parties without their knowledge or consent.

Examples: packet sniffing, or man-in-the-middle attacks.

##### Traffic Analysis

This involves the attacker analyzing network traffic patterns and metadata to gather information about the system, network, or device.

Here the intruder can’t read the message but only understand the pattern and length of encryption.

Traffic analysis can be performed using a variety of techniques, such as network flow analysis, or protocol analysis.

#### Active Attacks

Active attacks refer to types of attacks that involve the attacker actively disrupting or altering system, network, or device activity.

Active attacks are typically focused on causing damage or disruption, rather than gathering information or intelligence.

These are divided into four types based on their behaviour:

1. Masquerade
2. Replay
3. Modification of Message
4. Denial of Service

##### Masquerade

It is a type of attack in which the attacker pretends to be an authentic sender in order to gain unauthorized access to a system.

This type of attack can involve the attacker using stolen or forged credentials, or manipulating authentication or authorization controls in some other way.

##### Replay

It is a type of active attack in which the attacker intercepts a transmitted message through a passive channel and then maliciously or fraudulently replays or delays it at a later time.

##### Modification of Message

It involves the attacker modifying the transmitted message and making the final message received by the receiver look like it’s not safe or non-meaningful.

This type of attack can be used to manipulate the content of the message or to disrupt the communication process.

##### Denial of service

Also known as DoS attacks.

These attacks involve the attacker sending a large volume of traffic to a system, network, or device in an attempt to overwhelm it and make it unavailable to legitimate users.

### Security Mechanisms

The mechanism that is built to identify any breach of security or attack on the organization, is called a security mechanism.

Some examples of security mechanisms include:

#### Encryption

It involves the use of algorithms to transform data into a form that can only be read by someone with the appropriate decryption key.

Encryption can be used to protect data it is transmitted over a network, or to protect data when it is stored on a device.

#### Digital signature

It is a security mechanism that involves the use of cryptographic techniques to create a unique, verifiable identifier for a digital document or message, which can be used to ensure the authenticity and integrity of the document or message.

#### Traffic padding

It is a technique used to add extra data to a network traffic stream in an attempt to obscure the true content of the traffic and make it more difficult to analyze.

#### Routing control

It allows the selection of specific physically secure routes for specific data transmission and enables routing changes, particularly when a gap in security is suspected.

### Security Services

Security services refer to the different services available for maintaining the security and safety of an organization.

They help in preventing any potential risks to security.

Security services are divided into 5 types:

#### Authentication

It is the process of verifying the identity of a user or device in order to grant or deny access to a system or device.

#### Access control

It involves the use of policies and procedures to determine who is allowed to access specific resources within a system.

#### Data Confidentiality

It is responsible for the protection of information from being accessed or disclosed to unauthorized parties.

#### Data integrity

It is a security mechanism that involves the use of techniques to ensure that data has not been tampered with or altered in any way during transmission or storage.

#### Non repudiation

It involves the use of techniques to create a verifiable record of the origin and transmission of a message, which can be used to prevent the sender from denying that they sent the message.