# Basic Fundamentals

## Mathematical Operations in Networking
- Binary
  - prefix "bi"
  - exist in only two states "on" or "off" which is logically represented as "true" or "false" and mathematically a "1" or "0"
- Representation of binary information : Bits=1, Nibbles=4, Bytes=8, etc.
  - bit is a binary representation of the smallest set of information in a computer
  - A collection of these bits must be used to store large amounts of information

- Binary Types (# of bits)
  - 1 = bit/flag
  - 4 = nibble
  - 8 = byte/octet
  - 16 = half word
  - 32 = word (Ipadress)
  - 64 very long word
 
## Base(N) Formats
- Base 2 - Lowest level format and is the base language used by computer systems
  - 01000010 01100001 01110011 01100101 00100000 00110010
- Base 10 - Basis for the numbering system used by humans.
  - 66 97 115 101 32 49 48
- Base 16 - Used by computers and humans to express larger decimal numbers or long streams of binary into more manageable groupings
  - 42 61 73 65 20 31 36
- Base 16 - Used by computers and humans to express larger decimal numbers or long streams of binary into more manageable groupings
  - QmFzZSA2NA==

## Message Formatting Method
- Header
  - contains information related to control and communication processes between different protocol elements for different devices
- Data
  - This is the actual data being transmitted which contains the payload
- Footer
  - contents vary between communication methods or protocols
  - cyclical redundancy check (CRC) error-checking component is placed here

## Protocols and Interfaces
- Protocols
  - communications occurring at the same layer within the OSI model (horizontal)
  - Protocols allow communication to take place logically at layer 4 on two separate devices as if they were directly connected at layer 4
- Interfaces
  - information moving between different layers in the OSI model (vertical) on the same device

## Encapsulation and Decapsulation
- The communication between every layer other than the Physical layer is logical in nature. Therefore in order to pass information between protocol layers a protocol data unit (PDU) must be used.
- Each PDU for each protocol layer has specifications for the features or requirements at it’s layer
- The PDU is passed down to a lower layer for transmission, the next lower layer is providing the service of handling the previous layer’s PDU.

## OSI Model
![](![image](https://github.com/TJClarke58/Networking.md/assets/140441047/6bc69517-3ae9-44fc-8085-a49e2abc9f0a)
)

## Web Request Example
1. Application Layer:
   -  A user requests their browser to download a file from a web site. The HTTP protocol handler at this layer recognizes the secure request for a file on the web server and passes the request to the TLS Library.

2. Session and Presentation Layer:
   - The TLS library needs to create a secure channel for communication which requires the establishment of a connection with the destination. It passes a connection request to TCP.

3. Transport Layer:
   - The TCP handler receives the connection requests and creates a segment with the SYN flag (first part of the three-way handshake) set to the distant end server. The upper layer data is encapsulated with a TCP header and the segment is passed down to IP at the network layer.

4. Network Layer:
   - The network layer receives the segment and creates a packet by encapsulating the TCP header and data payload in an IP header. The correct IP information is added to the header for the destination IP address. The packet is then passed down to the Data-Link layer.

5. Data-Link Layer:
   - The data-link layer creates a frame encapsulating the data payload, TCP header, and IP header. The destination MAC address of the router is added to send the frame to it’s local destination. In addition to a header a trailer/footer is added containing a checksum and padding if needed to aid in frame synchronization. This is then passed to the physical layer.

6. Physical Layer:
  - The physical layer takes the binary data and injects it onto the physical media for which transmission is taking place (wireless or wired)

### OSI Layer, PDU, Common Protocols
- 7 - Application = Data = DNS, HTTP, TELNET
- 6 - Presentation = Data = SSL, TLS, JPEG, GIF
- 5 - Session = Data = NetBIOS, PPTP, RPC, NFS
- 4 - Transport = Segment/Datagram = TCP, UDP
- 3 - Network = Packet = IP, ICMP, IGMP
- 2 - Data Link = Frames = PPP, ATM, 802.2/3 Ethernet, Frame Relay
- 1 - Physical = Bits = Bluetooth, USB, 802.11 (Wi-Fi), DSL, 1000Base-T

## Internet Standards Organizations
- IETF
- IANA
- IEEE
