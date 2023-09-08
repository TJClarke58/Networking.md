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

## STREAM AND DATAGRAM SOCKET DEMOS
- 


