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

