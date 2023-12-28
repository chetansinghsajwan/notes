# Unit 4

### RTP

RTP stands for Realtime Transport Protocol.

RTP can be used for transporting common formats for sound and MPEG and H.263 for video.

It can also be used for transporting proprietary sound and video formats.

RTP typically runs on top of UDP. The sending side encapsulates a media chunk within an RTP packet, then encapsulates the packet in a UDP segment, and then hands the segment to IP. The receiving side extracts the RTP packet from the UDP segment, then extracts the media chunk from the RTP packet, and then passes the chunk to the media player for decoding and rendering.

RTP is a packet-based protocol, which means that it breaks the media stream into packets for transmission over the network.

RTP is used in conjunction with the Real-time Transport Control Protocol (RTCP), which is used to monitor the quality of the data transmission. RTP provides the actual delivery of the media, while RTCP is used to provide feedback on the quality of the transmission and to provide other control information.

###### Applications of RTP

1. RTP mainly helps in media mixing, sequencing and time-stamping.
2. Voice over Internet Protocol (VoIP)
3. Video Teleconferencing over Internet.
4. Internet Audio and video streaming.


### SIP

SIP stands for Session Intiation Protocol.

It was designed by IETF.

It can run on UDP and TCP.

It is a lightweight protocol that does the following:

- It provides mechanisms for establishing calls between a caller and a callee over an IP network.
- It provieds mechanisms for the caller to derermine the current IP address of the callee.
- It provides mechanisms for call management, such as adding new media streams during the call, changing the encoding during the call, inviting new participants during the call, call transfer, and call holding.

#### SIP Address

In SIP, the sender and receiver are often identified by any of these.

1. An Email address.
2. An IP address.
3. A Phone number

### H.323 Protocols

H.323 is a set of standards for real-time communication over IP networks.

It was developed by the International Telecommunication Union (ITU) in the late 1990s.

Some protocols that H.323 includes:

- **G.711**: Used for compression.
    
- **H.225**: Used for signaling, which is the process of establishing and maintaining communication sessions.
    
- **H.245**: Used for control, which is the process of managing the communication session.
    
- **RAS**: Used to establish and maintain connections between devices.
    
- **RTCP**: Used to control RTP channels.
    
- **RTP**: Used for actual data tranmission.

#### Differences between SIP and H.323

- H.323 is a complete, vertically integrated suite of protocols for multimedia conferencing, signalling, registration, admission control, transport, and codes. SIP addresses only session initiation and management and is a single component.

- H.323 comes from the ITU, whereas SIP comes from the IETF and borrows many concepts from the Web, DNS, and Internet e-mail.

- H.323 is large and complex. SIP uses the KISS principle: keep it simple, stupid.

## References

- https://www.tutorialspoint.com/h-323-and-associated-protocols

- https://www.geeksforgeeks.org/internet-telephony-protocol-h-323/