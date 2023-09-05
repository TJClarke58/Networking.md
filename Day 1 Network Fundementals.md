# Basic Fundamentals

## Mathematical Operations in Networking
- Binary
  - prefix "bi"
  - exist in only two states "on" or "off" which is logically represented as "true" or "false" and mathematically a "1" or "0"
- Representation of binary information : Bits=1, Nibbles=4, Bytes=8, etc.
  - bit is a binary representation of the smallest set of information in a computer
  - A collection of these bits must be used to store large amounts of information

- Binary Types (# of bits)TCP Header
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

## Physical Layer (OSI Layer 1)
- Physical Layer
  - the lowest layer of the OSI model
  - where data is physically sent across the network as ones and zeros
 
### Physical Layer Responsibilities
- Hardware Specifications
  - details of cables, connectors, NIC’s and other hardware.
- Encoding and Signaling
  - transforms data from bits to electrical or analogue signals that can be sent over a network
- Data Transmission and Reception
- Physical Network Design
  - LAN and WAN topologies
 
## Data Link Layer (OSI Layer 2)
- The data link layer is used when two hosts need to communicate across a common form of physical medium (fiber, copper, wireless, etc.)
- Addressing Schemes for LAN (physical addressing)
  - Used to differentiate between individual devices that share the same physical medium. Commonly MAC addressing.
- Error Notification
  - Provides error notification that alert protocols in the upper layers of errors that occur at the physical link. An example is loss of clocking signal between serial connections or signal loss for a wireless connection.
- Flow Control
  - Allows receiving devices on a link to detect congestion and notify neighboring devices so upper layer protocols can adjust the flow of traffic to prevent overflowing of buffers and frame drops.
- Frame Sequencing
  - Frame sequencing capabilities allow frames transmitted out of sequence to be re-ordered at the receiver. The integrity can be checked utilizing the frame check sequence field of the layer 2 header
 
## Data Link Sub-layers
- Data link layer is unique because it has the function to communicate in "logical" and "physical"
- two logical sub-layers
  - upper sub-layer, LLC, to interact with the network layer above
  - lower sub-layer, MAC, to interact with the physical layer
 
- MAC (Media Access Control)
  - Act as a sublayer governing protocol access to the physical medium, physical addressing, and acts as an interface between the LLC and physical layer. Most of the frame construction happens at this layer.
- LLC (Logical Link Control)
  - Manages communication between devices over a single link of the network that includes error checking and data flow. This layer provides the Ethertype to the MAC sublayer in the frame construction to identify the encapsulated protocol.

## Ethernet Header
- Structure:
  - Preamble (7 bytes)
    - Consists of alternating 1’s and 0’s to allow network synchronization with receiver clocks. Ethernet is self-clocked, the clock is extracted from the signal. This is stripped off at the NIC and not visible by packet analyzer software.
  - SFD (Start Frame Delimiter) (1 byte field)
    - Marks the end of the preamble, and beginning of the Ethernet frame. This is stripped off at the NIC and not visible by packet analyzer software.
- MAC Header (12 byte field)
  - Initial 6 bytes (48 bits) contain the Destination MAC address
  - Next 6 bytes (48 bits) contain the Source MAC Address
  - It is worth to note that this pretty much the only time that the destination address comes before the source. Most every other header we will deal with the source address will come first.
- Ethertype (2 byte field)
  - Used to indicate the next protocol encapsulated in the frame.This is provided by the LLC sub-layer.
    - Common Ethertypes controlled by IANA.org:
        - 0x0800 - IPv4
        - 0x0806 - ARP
        - 0x86DD - IPv6
        - 0x8100 - VLAN Tagging 802.1q
- Data / Payload (46-1500 byte field)
  - Consists of the encapsulated upper layer headers and data payload which may be 46-1500 bytes.
- FCS/CRC (Frame Check Sequence / Cyclical Redundancy Check) (4 byte field)
- Mathematical formula calculated on the entire frame.

## 802.1Q Header
- Structure:
  - MAC Header (12 byte field)
    - Initial 6 bytes contain the Destination MAC address
    - Next 6 bytes contain the Source MAC Address
  - VLAN Tag (4 byte field)
    - Tag Protocol ID (2 byte field)
      - Initial 2 bytes contain the new effective Ethertype field of 0x8100 indicating tagging
    - Tag Control Information (2 byte field)
      - Priority Code Point (3 bits)
      - Drop Eligible Indicator (1 bit)
    - VLAN ID (12 bit field)
  - Ethertype (2 byte field)
    - Used to indicate the next protocol encapsulated in the frame.
  - Data / Payload (46-1500 byte field)
    - Consists of the encapsulated upper layer headers and data payload which may be 46-1500 bytes
- FCS/CRC (Frame Check Sequence / Cyclical Redundancy Check) (4 byte field)
    - A new calculation is conducted to accommodate the addition of the new tag information. This calculation is done by the switch or router that added the tag. This also will be stripped off by the receiving NIC and will not be viable by the network analyzer.

## ARP Header (Student FG)
- The Address Resolution Protocol (ARP) is a Layer 2 protocol of the OSI model
  - ARP - To resolve the L2 (MAC) address when only the L3 (IPv4) address is known.
  - RARP - To resolve the L3 (IPv4) address when only the L2 (MAC) is known. (This protocol has been deprecated since the widespread use of protocols like BOOTP and DHCP.)
  - Proxy ARP - A device (router) answers the ARP queries for IP address that is on a different network. The ARP proxy see the ARP request and determines that the target Network address is not on the local network segment and is aware of how to reach the destination network. 
  - ARP Cache - is a collection of Layer 2 to Layer 3 address mappings discovered utilizing the ARP request/response process.
 
## Network Layer (OSI Layer 3)
- Addressing Schemes for Network (Logical Addressing)
  - Each device on the network has a logical addresses associated with it. This address is independent of the hardware device and must be unique in an internetwork.
- Routing
  - The moving of data across a series of interconnected networks is the job of devices and software that exist at this layer. The network layer must handle incoming packets from various sources, determine their final destination, and send them to the appropriate interface and forward devices to be processed and routed once again.
- Encapsulation
  - Encapsulation of messages received from higher layers must be performed to be passed on to the data-link layer.
- IP Fragmentation and Reassembly
  - Due to constraints on bandwidth and other limiting factors, the network layer must be able to fragment packets that are too large and re-assemble the data in order at the destination device.
- Error Handling and Diagnostics
  -The network layer uses special helper protocols like ICMP and ARP that allow logically connected devices to exchange information about the status of the network or devices themselves.

## Internet Protocol Versions
- The network layer deals in two version of IP and ICMP, version 4 and version 6
- IP layer protocols to include IPv4, IPv6, ICMP, IGMP, and various Routing Protocols
- IPv6 features, addressing etc.

## IPv4 Header
(Field Name  -  Bit Range  -  Length 	-  Description)
1. Version  -  0-3  -   4 bits  -  Represents the IP version of the packet—Always 4 in IPv4.
2. Internet Header Length  -  4-7  -  4 bits  -  Represents the number of 32-bit (4-byte) words in the header. If IHL is greater than 5, it indicates that IP options are present.
3. Differentiated Service Code Point (Multiply by 4 to get ECN)  -  8-13  -  6 bits  -  Indicates the type of traffic that is being passed (e.g., VoIP, Video).
4. Explicit Congestion Notification  -  14-15  -  2 bits  -  ECN is used to provide end-to-end traffic congestion notification; usually 0.
5. Total Length  -  16-31  -  16 bits  -  Represents the size of the entire IP packet in bytes, including the IP header. The minimum value would be 20 (0x0014, in hex). The max value is 65,535.
6. Identification  -  32-47  -  16 bits  -  Used to identify fragmented packets.
7. Flags  -  48-50  -  3 bits  -  The flags field is used for fragmentation of IP packets. The first bit (at offset 48) is reserved and must be zero. The second bit (at offset 49) is the Don’t-Fragment flag. The third bit (at offset 50) is the More-Fragments flag.
8. Fragment Offset  -  51-63  -  13 bits  -  Identifies the offset of a fragmented packet’s position, relative to the original packet, in multiples of 8 bytes.
9. Time To Live  -  64-71  -  8 bits  -  Determines the maximum number of times a packet may be forwarded. Each router that forwards a packet decrements the TTL by one.
10. Protocol  -  72-79  -  8 bits  -  Identifies the protocol header that is used in the encapsulated payload. Uses IP protocol codes, e.g., TCP = 6 (0x06).
11. Header Checksum  -  80-95  -  16 bits  -  The checksum is calculated from the IP header only. Can be used to indicate tampering or corruption.
12. Source Address  -  96-127  -  32 bits  -  Source IPv4 address of the packet.
13. Destination Address  -  128-159  -  32 bits  -  Destination IPv4 address of the packet.
14. Options  -  160-256  -  128 bits  -  Options present if IHL > 5. Can range from 6 to 15 WORDS

## Fragmentation Process
- IP fragmentation breaks up a single IPv4 packet into multiple smaller packets. Every link in a network has a defined maximum transmission unit (MTU).
- In IPv4, routing devices perform fragmentation if the total size of the packet (header and data) coming from one network interface is greater than the MTU of the network out the exiting interface
- IPv4 Flags: Starting at byte offset 6, A 3 bit field declares if the packet is a part of a fragmented data frame or not.
  - Bit 0: reserved, should always be 0. See RFC 3514 for a description of the “evil bit.”
  - Bit 1: 0 = May Fragment, 1 = Don’t Fragment this packet
  - Bit 2: 0 = Last Fragment, 1 = More Fragments follow.
- Fragmantation math to find offset
  - (MTU - IHL) / 8 = OFFSET (IN BYTES)

## IPv6 Header (LOOK AT STUDENT FG)

## Fingerprinting
- Sometimes you can analyze a header and make an educated guess at which operating system sent the packet by your TTL maximum hops.

## ICMP Header (LOOK AT STUDENT FG)

## ZERO CONFIGURATION
- IPv4 Auto Configuration
  - * APIPA
  - * RFC 3927
- IPv6 auto configuration
  - * SLAAC (StateLess Address Auto-configuration)
  - * RFC 4862

## Transport Layer (OSI Layer 4)
- Connection-oriented (TCP-Segments-Unicast traffic)
  - requires that a connection with specific agreed-upon parameters be established before data is sent.
  - provides segmentation and sequencing.
  - provides connection establishment and acknowledgments.
  - provides flow control (or windowing).
  - Identify common application layer protocols or functions that rely on TCP.
- Connection-less (UDP-Datagrams-Broadcast, Multicast, Unicast Traffic)
  - requires no connection before data is sent
  - provides no ordering, duplicate protection or delivery guarantee
  - does provide integrity checking
  - Identify common application layer protocols or functions that rely on UDP

## TCP Header
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/80397692-e6de-438d-8b84-e043aa0da211)

## TCP Flags 
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/fd6e02b4-3efc-4015-8934-a47227c273cb)

## TCP States
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/0556ab73-c7e0-4880-975f-ddd5a9411a9d)

## TCP Connections
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/39641f55-bebc-4463-89e5-e55b601655fc)
- Connection establishment
  - 3-way Handshake
    - SYN, SYN-ACK, ACK
- Data Transfer
  - During the ESTABLISHED state communication can go in either direction. Data can be either set to or pulled from the server
    - PSH-ACK (Server is sending data to client so it turns on the PSH flag)
    - ACK
- Connection Termination
  - 4-way termination.
    - FIN-ACK - Initiator will set the FIN flag to inform the other end that it is closing its end of the connection. It will set its SEQ number to the next incrementing number.
    - ACK - Receiver will set the ACK flag and ACK’s the initiator’s SEQ number +1 in the ACK field.
    - FIN-ACK - Receiver then initiates its connection termination buy setting the FIN flag and setting its own SEQ number in the SEQ field.
    - ACK - Original initiator then sets the ACK flag and ACK’s the receiver’s SEQ number +1 in the ACK field. SEQ number will be the next incrementing SEQ number.

- TCP Options:
  - Option 0 - End of Options List
  - Option 1 - No Options or NOP
  - Option 2 - Maximum Segment Size (MSS)
  - Option 3 - TCP Window Scaling

## UDP Header
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/a5a9e3f6-262b-4c28-8c4d-4f0b1a9afee1)

## Session Layer (OSI Layer 5)
- Session Layer — The main purpose of this layer is to maintain the state of your ongoing connections. This state is not used in a connection-less protocol.
  - This layer￼ provides the capabilities to open, close and manage sessions between the application layer processes
  - The communication at this layer consist of requests and responses that occur between the local and remote applications
  - Session-layer makes use of remote procedure calls (RPCs), Net-Beui, SOCKS, SMB, WINS, named-pipes, PPTP and other protocols

## Protocols

### Socks 4/5 (TCP 1080) (SOCK 5 SUPPORTS UDP)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/f9ca7c74-7283-4846-9edd-65d5f5b9b6f4)
- Initiates connections through a proxy
- Uses various Client / Server exchange messages
- Client can provide authentication to server
- Client can request connections from server

### PPTP (TCP 1723)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/969e34cd-c331-49e2-af5b-e8fa2c280daf)

### L2TP (TCP 1701)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/e003f7f6-4d97-48fa-966b-c7573bc27b91)

### SMB/CIFS (TCP 139/445 AND UDP 137/138)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/d4b101fa-327c-44ce-bcee-667a142c9d15)
- Allowed devices to establish connections to other devices on network to share files, printers and other things.
- SMB Rides over Netbios - allows applications to communicate over a LAN using a NetBIOS name. Depricated due to DNS. Netbios Wiki
  - Netbios Dgram Service - UDP 138
  - Netbios Session Service - TCP 139
- SAMBA and CIFS are just flavors of SMB

### RPC (Any Port)
- RPC is a request/response protocol.
- User application will:
  - Sends a request for information to a external server
  - Receives the information from the external server
  - Display collected data to User

## Presentation Layer (OSI Layer 6)
- Presentation Layer — This layer deals with the Translating, Formatting, Encryption, and Compression of data.
  - Translation - The presentation layer is responsible for interoperability between encoding methods as different computers use different encoding methods. It translates data between the formats the network requires and the format at the computer.
  - Formatting - This layer is responsible to put the file in a format that is readable. Such as:
    - ASCII or EBCDIC
    - .doc, .ppt or .xls
    - mp3 or wav
    - avi or mp4
    - bmp, jpeg, gif, tiff or png
  - Encryption This is the layer the encryption and decryption gets carried out.
    - Symetric: AES, Blowfish, Twofish, DES, and RC4
    - Asymetric: PKI, Deffie-Hellman, DSS, RSA, Elliptic curve
  - Compression Sometimes data gets to big to transmit over the network so The Presentation layer handles compression.The primary role of Data compression is to reduce the number of bits to be transmitted. It is important in transmitting multimedia such as audio, video, text etc.
    - Zip, TAR, RAR, 7zip, CAB

## Application Layer (OSI Layer 7)
- FTP (TCP 20/21)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/42f1fd78-da2e-4b82-ae42-34f38efb9882)
- FTP
  - File Transfer Protocol is a standard network protocol that is used for file transfer between a client and a server.

## FTP Active
- Active
  - A client initiates a connection with a server on port 21 from the client’s ephemeral high port.
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/678790cf-5677-4fd0-bb7a-9dc927bad5de)

## FTP Passive
- Passive
  - Passive FTP sidesteps the issue of Active mode by reversing the conversation. The client initiates both the command and data connections.
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/5a6f2615-a957-4f2d-a727-38815056a8bb)

## SSH (TCP 22)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/a3a78447-b858-4409-9bb1-e64b3f2fdbb1)
- Messages provide:
  - Client/server authentication
  - Asymmetric or PKI for key exchange (Encyrpting the session)
  - Symmetric for session (actual conversation key) 
  - User authentication
  - Data stream channeling

## SSH Architecture
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/60768d40-247f-42d4-acc0-dc38e57b8e87)
- Server = Known as sshd in most linux SSH implementations, this allows incoming SSH connections and handles authentication and authorization
- Client = This is the program that connects to the SSH server for a request, examples include scp and ssh
- Session = The client and server conversation that begins after successful mutual authentication.
- Keys
  - User Key - Asymmetric public key used to identify the user to the server
  - Host Key - Asymmetric public key used to identify the server to the user
  - Session Key - Symmetric key created by the client and server to protect the session’s communication.
- Key Generator = Creates user keys and host keys via ssh-keygen
- Known-hosts database = Collection of host keys that the client and server refer to for mutual authentication.
- Agent = Stores keys in memory as a convenience for users to not input pass-phrases repetitively.
- Signer = This is a program that signs the host-based authentication packets.
- Random Seed = Random data used for entropy in creating pseudo-random numbers
- Configuration File = Settings that exist on either the client or server that dictate functionality for ssh or sshd respectively

## SSH IMPLEMENTATION CONCERNS
- Using password authentication only
- Key rotation
- Key management
- Implementation specification (libssh, sshtrangerthings)

## TELNET (TCP 23)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/3137b621-3c54-4e3c-8e2a-6ae4bf650b86)
- Messages:
  - Telnet Commands
  - Telnet options

## SMTP (TCP 25)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/705d3e73-ca08-455e-99bb-08c06696ce09)
- Messages:
  - SMTP Commands
  - SMTP Responses

## TACACS (TCP 49) SIMPLE/EXTENDED
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/434b2925-bdfc-4305-9a35-7aa470a78239)

## HTTP(S) (TCP 80/443)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/ca5d0996-9f8f-482b-b937-7ba87b682eb1)
- Messages:
  - Methods
    - GET / HEAD / POST / PUT
  - HTTP status Codes
    - 100, 200, 300, 400

## POP (TCP 110)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/cb20feca-6a10-4b97-a386-24fd0b7472ed)
- Messages:
  - POP Commands
  - POP Replies
  - POP Capabilities

## IMAP (TCP 143)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/617684aa-b746-4b8d-baaf-3120db86c9b8)
- Messages:
  - IMAP Commands
  - IMAP Status Response
  - IMAP Capabilities

## RDP (TCP 3389)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/5a075341-3af6-442a-bb97-3bf693ec1f76)
- Compression or Encryption support
- Desktop size and color depth
- Keyboard Mapping
- Remote system control
- Mouse-cursor color properties
  
## DNS (QUERY/RESPONSE) (TCP/UDP 53)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/2559232a-8b82-4f30-bfe5-c07806694dda)

## DHCP (UDP 67/68)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/bfef2bbf-32fc-43aa-840b-6717bcdffe7d)
- Discover
- Offer
- Request
- Acknowledge

## TFTP (UDP 69)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/622c8642-4354-4ae8-8453-bffb3a0b36c9)
- Messages:
  - TFTP Opcodes
  - TFTP Error Codes

## NTP (UDP 123)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/d1ae822b-19c2-48d3-9723-8d536c4f46df)

## SNMP (UDP 161/162)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/8fd97f58-cfc8-4e4e-ac33-8b8c51f0fd65)
- 7 Message Types:
  - Get Request
  - Set Request
  - Get Next
  - Get Bulk
  - Response
  - Trap
  - Inform

## RADIUS (UDP 1645/1646 AND 1812/1813)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/3c422ad8-c8ad-4f6c-ae82-6490bcfce687)

## RTP (UDP any above 1023)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/e203f9cf-b969-4b28-99c5-efefd48c64cd)

