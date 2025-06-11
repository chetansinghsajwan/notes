# SSH Protocol

SSH is a protocol to securely connect to a remote computer over an unsecured network.

It is defined by [IETF](ietf/ietf). It is defined in the following [RFCs](ietf/rfc):

- RFC 4251 - SSH Protocol Architecture
- RFC 4252 - Authentication
- RFC 4253 - Transport Layer
- RFC 4254 - Connection Protocol

It consists of three major components:

- The Transport Layer Protocol [SSH-TRANS] provides server authentication, confidentiality, and integrity.  It may optionally also provide compression.  The transport layer will typically be run over a TCP/IP connection, but might also be used on top of any other reliable data stream.

- The User Authentication Protocol [SSH-USERAUTH] authenticates the client-side user to the server.  It runs over the transport layer protocol.

- The Connection Protocol [SSH-CONNECT] multiplexes the encrypted tunnel into several logical channels.  It runs over the user authentication protocol.


## References

- https://grahamhelton.com/blog/ssh-cheatsheet/
