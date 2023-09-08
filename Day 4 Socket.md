# Day 4 Packet and Socket Creation

## SOCKET TYPES
- Stream Sockets (TCP)
  - Connection oriented and sequenced; methods for connection establishment and tear-down. Used with TCP, SCTP, and Bluetooth.
- Datagram Sockets (UDP)
  - Connectionless; designed for quickly sending and receiving data. Used with UDP.
- Raw Sockets (WHATEVER NEEDED
  - Direct sending and receiving of IP packets without automatic protocol-specific formatting.

## USER SPACE VS. KERNEL SPACE SOCKETS
- User Space Sockets (User space is a portion of system memory where a user’s processes can run)
  - Stream Sockets
  - Datagram Sockets
- Kernel Space Sockets (Kernel space is where the operating system runs and provides its services)
  - Raw Sockets
 
## SOCKET CREATION AND PRIVILEGE LEVEL
- User Space Sockets
  - The most common sockets that do not require elevated privileges to perform actions on behalf of user applications.
- Kernel Space Sockets
  - Attempts to access hardware directly on behalf of a user application to either prevent encapsulation/decapsulation or to create packets from scratch, which requires elevated privileges.

## USING SPACE APPLICATIONS/SOCKETS
- Using tcpdump or wireshark to read file
- using nmap with no switches
- Using netcat to connect to a listener
- Using netcat to create a listener above the well known port range(1024+)
- Using /dev/tcp or /dev/udp to transmit data

## KERNEL SPACE APPLICATIONS/SOCKETS
- Using tcpdump or wireshark to capture packets on the wire
- Using nmap for OS identification or to set specific flags when scanning
- Using netcat to create a listener in the well known port range (0 - 1023)
- Using Scapy to craft or modify a packet for transmission
- Using Python to craft or modify Raw Sockets for transmission
- Network devices using routing protocols such as OSPF

## UNDERSTANDING PYTHON TERMINOLOGY
- Libraries
  - Modules (define functions, classes and variables)
    - Functions (a block of organized, reusable code that is used to perform a single, related action.)
    - Exceptions (a special condition encountered during program execution that is unexpected or anomalous)
    - Constants (data or a value that does not change in a specified amount of time, unlike a variable)
    - Objects (section of code used in object-oriented programming that can be used by other object modules or the program being created)
    - Types

## NETWORK PROGRAMMING WITH PYTHON3
- Network sockets primarily use the Python3 Socket library and socket.socket function.
  - import socket
```
s = socket.socket(socket.FAMILY, socket.TYPE, socket.PROTOCOL)
```

## THE SOCKET.SOCKET FUNCTION
- Inside the socket.socket. function, you have these arguments, in order:
```
socket.socket([*family*[,*type*[*proto*]]])
```
- family constants should be: AF_INET (default), AF_INET6, AF_UNIX (What we are using in class)
- type constants should be: SOCK_STREAM (default), SOCK_DGRAM, SOCK_RAW
- proto constants should be: 0 (default), IPPROTO_RAW

## Socket API Functions
- socket() creates a new socket of a certain type, identified by an integer number, and allocates system resources to it.
- bind() is typically used on the server-side, and associates a socket with a socket address structure, i.e. a specified local IP address and a port number.
- listen() is used on the server-side, and causes a bound TCP socket to enter listening state.
- connect() is used on the client-side, and assigns a free local port number to a socket. In the case of a TCP socket, it causes an attempt to establish a new TCP connection.
- accept() is used on the server-side. It accepts a received incoming attempt to create a new TCP connection from the remote client, and creates a new socket associated with the socket address pair of this connection.
- send(), sendall(), recv(), sendto(), and recvfrom() are used for sending and receiving data. The standard functions write() and read() may also be used.
- close() causes the system to release resources allocated to a socket. In case of TCP, the connection is terminated.
- gethostbyname() and gethostbyaddr() are used to resolve hostnames and addresses. IPv4 only.
- select() is used to suspend, waiting for one or more of a provided list of sockets to be ready to read, ready to write, or that have errors.
- poll() is used to check on the state of a socket in a set of sockets. The set can be tested to see if any socket can be written to, read from or if an error occurred.
- getsockopt() is used to retrieve the current value of a particular socket option for the specified socket.
- setsockopt() is used to set a particular socket option for the specified socket.

## RAW IPV4 SOCKETS
- Raw Socket scripts must include the IP header and the next headers.
- Requires guidance from the "Request for Comments" (RFC)to follow header structure properly.
  - RFCs contain technical and organizational documents about the Internet, including specifications and policy documents.
- See RFC 791, Section 3 - Specification for details on how to construct an IPv4 header.

## RAW SOCKET USE CASE
- Testing specific defense mechanisms - such as triggering and IDS for an effect, or filtering
- Avoiding defense mechanisms
- Obfuscating data during transfer
- Manually crafting a packet with the chosen data in header fields

## ENCODING AND DECODING
- Encoding
  - The process of taking bits and converting them using a specified cipher.
- Decoding
  - Reverse of the conversion process used by the specified cipher for encoding.
- Common encoding schemes
  - UTF-8 (default), Base64, Hex
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/1d2f2f72-d824-43f0-ad72-4f2ba92b90ad)
