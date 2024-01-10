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

## Conventional Encryption Model

It is a cryptographic system that uses the same key used by the sender to encrypt the message and by the receiver to decrypt the message.

It was the only type of encryption in use prior to the development of public-key encryption.

It has mainly 5 components:

1. Plain text
2. Encryption algorithm 
3. Secret key
4. Ciphertext 
5. Decryption algorithm

### Advantages

- Simple

- Fast

- Use fewer computer resources.

### Disadvantages

1. Origin and authenticity of the message cannot be guaranteed, since both sender and receiver use the same key, messages cannot be verified to have come from a particular user.

2. It isn’t much secured when compared to public-key encryption.

3. If the receiver lost the key, he/she cant decrypt the message and thus making the whole process useless.

4. This scheme does not scale well to a large number of users because both the sender and the receiver have to agree on a secret key before transmission.

## Cryptanalysis

It is the study of the cryptographic algorithm and the breaking of those algorithims.

To determine the weak points of a cryptographic system, it is important to attack the system. This attacks are called cryptanalytic attacks.

Some cryptanalysis attacks:

- **Known-Plaintext Analysis (KPA)**
    
    In this type of attack, some plaintext-ciphertext pairs are already known. Attacker maps them in order to find the encryption key. This attack is easier to use as a lot of information is already available.

- **Chosen-Plaintext Analysis (CPA)**
    
    In this type of attack, the attacker chooses random plaintexts and obtains the corresponding ciphertexts and tries to find the encryption key. Its very simple to implement like KPA but the success rate is quite low.

- **Ciphertext-Only Analysis (COA)**
    
    In this type of attack, only some cipher-text is known and the attacker tries to find the corresponding encryption key and plaintext. Its the hardest to implement but is the most probable attack as only ciphertext is required.

- **Man-In-The-Middle (MITM) attack**
    
    In this type of attack, attacker intercepts the message/key between two communicating parties through a secured channel.

- **Birthday attack**
    
    This attack exploits the probability of two or more individuals sharing the same birthday in a group of people. In cryptography, this attack is used to find collisions in a hash function.

- **Brute-force attack**
    
    This attack involves trying every possible key until the correct one is found. While this attack is simple to implement, it can be time-consuming and computationally expensive, especially for longer keys.

## Steganography

It is a method of hiding secret data, by embedding it into an audio, video, image, or text file.

###### How is it different from cryptography?

Cryptography and steganography are both methods used to hide or protect secret data. However, they differ in the respect that cryptography makes the data unreadable, or hides the meaning of the data, while steganography hides the existence of the data.

###### Image Steganography

As the name suggests, Image Steganography refers to the process of hiding data within an image file. The image selected for this purpose is called the cover image and the image obtained after steganography is called the stego image.