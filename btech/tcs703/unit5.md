# Unit 5

![](unit5-img-1.png)

## Socket Address Structure

###### Posix Definition

```c
struct in_addr {
  in_addr_t s_addr; // 32 bit IPv4 network byte ordered address
};

struct sockaddr_in {
  uint8_t sin_len; // length of structure (16)
  sa_family_t sin_family; // AF_INET
  in_port_t sin_port; // 16 bit TCP or UDP port number
  struct in_addr sin_addr; // 32 bit IPv4 address
  char sin_zero[8]; // not used but always set to zero
};
```

###### Generic Definition

```c
struct sockaddr {
  uint8_t sa_len;
  sa_family_t sa_family; // address family: AD_xxx value
  char sa_data[14]; // unused data to complete size requirements
};
```

### Byte Order

Different cpu architectures use different byte orders, little-endian and big-endian. But the network protocols work on a specific byte order.

The byte order of the host architecture is called host byte byte order.

The byte order used by the network protocol is called network byte order.

To perform conversions between host and network byte order, following functinos are used.

```c
#include <netinet/in.h>

// host to network short
uint16_t htons(uint16_t val);

// host to network long
uint32_t htonl(uint32_t val);

// network to host short
uint16_t ntohs(uint16_t val);

// network to host long
uint32_t ntohl(uint32_t val);
```

## TCP Connection Procedure

Steps for establishing a connection on the client side are:

- Create a socket using the socket() function.
- Connect the socket to the address of the server using the connect() function.
- Send and receive data by means of the read() and write() functions.

Steps for establishing a connection on the server side are:

- Create a socket with the socket() function.
- Bind the socket to an address using the bind() function.
- Listen for connections with the listen() function.
- Accept a connection with the accept() function system call. This call typically blocks until a client connects with the server.
- Send and receive data by means of send() and receive().

## Functions

###### `socket` function

```c
#include "sys/socket.h"

///
/// # Returns
/// 
/// A non negative integer representing socket descriptor, or -1 on error.
int socket(int family, int type, int protocol);
```

###### `connect` function

```c
#include "sys/socket.h"

/// Establishes a connection with a tcp server.
/// 
/// # Parameters
/// 
/// - `sockfd`: The socket descriptor returned by the socket function.
/// 
/// # Returns
/// 
/// 0 if connection is established, else -1.
int connect(int sockfd, cosnt struct sockaddr* servaddr, socklen_t addrlen);
```

###### `bind` function

```c
#include "sys/socket.h"

/// 
/// 
int bind(int sockfd, const struct sockaddr* servaddr, socklen_t addrlen);
```

## Concurrent Servers

There are two main classes of servers:

1. Iterative Server
2. Concurrent Server

###### Iterative Servers

Iterative server iterates through each client and handles them one at a time.

They are simple to implement and fast for a small client as there is no shared resource handling logic.

###### Concurrent Servers

Concurrent serve handles each client concurrently.

They are implemented either by creating a new process using `fork()` function or creating a new thread.

---

###### Example

```c
pid_t pid;
int listenfd, connfd;

// fill the socket address with server’s well known port
listenfd = socket(...);
bind(listenfd, ...);
listen(listenfd, ...);

while (true) {
  // blocking call
  connfd = accept(listenfd, ...);
  
  if ( (pid = fork()) == 0 ) {
    // child closes listening socket
    close(listenfd);
    
    // process the request doing something using connfd
    // ...
    // ...
    
    close(connfd);
    
    // child terminates
    exit(0);
  }
  
  close(connfd); /*parent closes connected socket*/
}
```

When a connection is established, accept returns, the server calls fork, and the child process services the client (on the connected socket connfd).

The parent process waits for another connection (on the listening socket listenfd.

The parent closes the connected socket since the child handles the new client.

---

###### Echo Server

An echo server is an application which is used to test if the connection between a client and a server is successful. It consists of a server which sends back whatever text the client sent.

### Lack Of Flow Control With UDP

###### TCP iterative echo server implementation

```c
#include <stdlib.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>
#include <unistd.h>

#define MAXLINE 4096 /*max text line length*/
#define SERV_PORT 3000 /*port*/
#define LISTENQ 8 /*maximum number of client connections */

int main(int argc, char **argv)
{    
    // creating the socket
    int listenfd = socket(AF_INET, SOCK_STREAM, 0);
    if (listenfd < 0)
    {
        perror("Problem in creating the socket");
        exit(2);
    }
    
    // preparation of the socket address
    struct sockaddr_in servaddr;
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(SERV_PORT);
    bind(listenfd, (struct sockaddr*) &servaddr, sizeof(servaddr));
    
    listen(listenfd, LISTENQ);
    printf("server is running...");
    printf("waiting for connections...");
    
    while (true)
    {
        char buf[MAXLINE];
        socklen_t clilen = sizeof(cliaddr);
        struct sockaddr_in cliaddr;
        int connfd = accept(listenfd, (struct sockaddr*) &cliaddr, &clilen);
        printf("received request...");
        
        int n;
        while ((n = recv(connfd, buf, MAXLINE,0)) > 0)
        {
            printf("message recieved from client: ");
            puts(buf);
            printf("sending message back...");
            send(connfd, buf, n, 0);
        }
        
        if (n < 0)
        {
            perror("message read error.");
            exit(1);
        }
        
        close(connfd);
    }
    
    // close listening socket
    close(listenfd);
}
```

###### TCP concurrent echo server implementation

```c
#include <stdlib.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>
#include <unistd.h>

#define MAXLINE 4096 /*max text line length*/
#define SERV_PORT 3000 /*port*/
#define LISTENQ 8 /*maximum number of client connections*/

int main (int argc, char **argv)
{    
    // creating the soclet
    int listenfd = socket(AF_INET, SOCK_STREAM, 0);
    if (listenfd < 0)
    {
        perror("Problem in creating the socket");
        exit(2);
    }
    
    // creating the socket address
    struct sockaddr_in servaddr;
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(SERV_PORT);
    bind(listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr));
    
    // listen to the socket by creating a connection queue, then wait for clients
    listen(listenfd, LISTENQ);
    printf("server is running...");
    printf("waiting for connections...");

    while (true)
    {
        char buf[MAXLINE];
        
        // accept a connection
        struct sockaddr_in cliaddr;
        socklen_t clilen = sizeof(cliaddr);
        int connfd = accept(listenfd, (struct sockaddr*)&cliaddr, &clilen);
        printf("received request...");
        
        if (fork() == 0)
        {
            printf ("child created for dealing with client requests");
            
            // close listening socket
            close (listenfd);
            
            int n;
            while ((n = recv(connfd, buf, MAXLINE,0)) > 0)
            {
                printf("%s","String received from and resent to the client:");
                puts(buf);
                send(connfd, buf, n, 0);
            }
            
            if (n < 0)
            {
                printf("message read error.");
                exit(0);
            }
        }

        // close socket of the server
        close(connfd);
    }
}
```

###### TCP client implementation

```c
#include <stdlib.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>
#include <arpa/inet.h>

#define MAXLINE 4096 /*max text line length*/
#define SERV_PORT 3000 /*port*/

int main(int argc, char **argv)
{
    char sendline[MAXLINE], recvline[MAXLINE];
    
    // basic check of the arguments additional checks can be inserted
    if (argc !=2)
    {
        perror("Usage: TCPClient <IP address of the server");
        exit(1);
    }
    
    // create a socket for the client
    // if sockfd<0 there was an error in the creation of the socket
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd < 0)
    {
        perror("problem in creating the socket.");
        exit(2);
    }
    
    // creation of the socket
    struct sockaddr_in servaddr;
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr= inet_addr(argv[1]);
    servaddr.sin_port = htons(SERV_PORT); //convert to big-endian order
    
    // connection of the client to the socket
    if (connect(sockfd, (struct sockaddr *) &servaddr, sizeof(servaddr))<0)
    {
        perror("Problem in connecting to the server");
        exit(3);
    }
    
    while (fgets(sendline, MAXLINE, stdin) != NULL)
    {
        send(sockfd, sendline, strlen(sendline), 0);
        if (recv(sockfd, recvline, MAXLINE,0) == 0)
        {
            perror("the server terminated prematurely.");
            exit(4);
        }
        
        printf("message received from the server: ");
        fputs(recvline, stdout);
    }
    
    exit(0);
}
```

###### Localhost execution of client and server

**server**
```shell
# compile the server
$ gcc -o echo-server echo-server.c

# run the server
$ ./echo-server
server is running...
waiting for connections...

# after client connected

recieved request...
message recieved from client: hello!
sending message back...
```

**client**
```shell
# compile the client
$ gcc -o echo-client echo-client.c

# connect using the localhost address 127.0.0.1 and then type something
$ ./echo-client 127.0.0.1
message to sent to server: hello!
message received from the server: hello!
```

##### Remote execution of client and server

Remote execution procedure is exactly the same, just enter the server's address instead of the local host.

To get the address of the server:

```shell
# ssh into server
$ ssh <server>
<server>'s password: 

# get the name of the host
$ hostname
myhost

# get the address of the host
$ host myhost
myhost has address 129.170.213.32
...
```