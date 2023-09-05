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

