# Unit 5

## Firewall

It establishes a barrier between secured internal networks and outside untrusted network, such as the Internet.

It is a network security device, either hardware or software-based, which monitors all incoming and outgoing traffic and based on a defined set of security rules it accepts, rejects or drops that specific traffic.

**Accept**: allow the traffic
**Reject**: block the traffic but reply with an “unreachable error”
**Drop**: block the traffic with no reply

Firewall match the network traffic against the rule set defined in its table. Once the rule is matched, associate action is applied to the network traffic.

Firewalls are generally of two types:

1. Host-based
2. Network-based

#### Host-based Firewalls

Host-based firewall is installed on each network node which controls each incoming and outgoing packet.

It is a software application or suite of applications, comes as a part of the operating system.

Host-based firewalls are needed because network firewalls cannot provide protection inside a trusted network.

Host firewall protects each host from attacks and unauthorized access.

#### Network-based Firewalls

Network firewall function on network level. In other words, these firewalls filter all incoming and outgoing traffic across the network.

It protects the internal network by filtering the traffic using rules defined on the firewall.

A Network firewall might have two or more network interface cards (NICs).

A network-based firewall is usually a dedicated system with proprietary software installed.

### Advantages

1. Protection from unauthorized access

2. Control of network access

4. Monitoring of network activity

5. Regulation compliance

6. Network segmentation
    
    By using firewalls to split up a bigger network into smaller subnets, the attack surface is reduced and the security level is raised.

### Disadvantages

1. Complexity
    
    Setting up and keeping up a firewall can be time-consuming and difficult, especially for bigger networks or companies with a wide variety of users and devices.

2. Limited Visibility
    
    Firewalls may not be able to identify or stop security risks that operate at other levels, such as the application or endpoint level, because they can only observe and manage traffic at the network level.

3. Performance impact

4. Limited scalability
    
    Because firewalls are only able to secure one network, businesses that have several networks must deploy many firewalls, which can be expensive.

5. Limited VPN support
    
    Some firewalls might not allow complex VPN features like split tunneling, which could restrict the experience of a remote worker.

6. Cost
    
    Purchasing many devices or add-on features for a firewall system can be expensive, especially for businesses.

### Applications

1. Corporate networks

2. Government organizations

3. Service providers, such as VPN and load balancing. 

5. Networks at home

## Wireless Security

The 7 most common wireless network threats are:

1. Configuration Problems: Misconfigurations, incomplete configurations.
    
2. Denial of Service: Sending large amounts of traffic (or viruses) over the network with the intent of hijacking resources or introducing backdoors.
    
3. Passive Capturing: Eavesdropping within range of an access point to capture sensitive information.
    
4. Rogue (or Unauthorized/Ad-Hoc) Access Points: Fool devices into connecting with a false access point. 
    
5. Evil Twin Attacks: Impersonating legit access points with a stronger signal to entice authorized users to sign on. 
    
6. Hacking of Lost or Stolen Wireless Devices: Bypassing the password to gain access.
    
7. Freeloading: Piggybacking on a connection or intercepting file sharing.