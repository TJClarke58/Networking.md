# Day 3 Network Traffic Sniffing 

## CAPTURE LIBRARY
- What makes traffic capture possible?
  - Libpcap = Linux 
  - WinPcap = Windows
  - NPCAP = Both
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/c5d12c60-0143-4650-ad99-47f2463ebccc)

# WIRESHARK, TSHARK, TCPDUMP AND BPFS
- TCPDump Demo on Internet_Host
- TCPDump is a tool used to capture and display the contents of packets traversing a network interface TCPDump uses native and/ or BPF filter
  - Macro:
    - <macro> <value>
    - not port 22
  - BPF:
    - <protocol header> [offset:length] <relation> <value>
    - tcp[2:2] !=22
- tcpdump -D
  - -D Print the list of the network interfaces available on the system and on which TCPDump can capture packets.
- tcpdump -i eth0
  - -i Normally, eth0 will be selected by default if you do not specify an interface.
- tcpdump -i eth0 -X
  - -X displays packet data in HEX and ASCII.
- tcpdump -i eth0 -XX
  - -XX displays the packet data in HEX and ASCII to include the Ethernet portion.
- tcpdump -w something.pcap
  - -w writes the capture to an output file
- tcpdump -vv
  - -v gives more verbose output with details on the time to live, IPID, total length, options, and flags. Additionally it enables integrity checking.
- tcpdump -r something.pcap
  - -r reads from the pcap

## Logical Operators:
- AND (&&)
- OR (||)
- NOT (!)
Example: sudo tcpdump -i eth0 -XXn 'not port 22 && not port 23'

## Demonstrate TCPDump syntax with filters and logical operators:
- tcpdump for specific protocol traffic of more than one type.
  - tcpdump port 80 or 22 -vn
- tcpdump for range of ports on 2 different hosts with a destination to a specific network
  - tcpdump portrange 20-100 and host 10.1.0.2 or host 10.1.0.3 and dst net 10.2.0.0/24 -vn
- tcpdump filter for source network 10.1.0.0/24 and destination network 10.3.0.0/24 or dst host 10.2.0.3 and not host 10.1.0.3.
  - tcpdump "(src net 10.1.0.0/24  && (dst net 10.3.0.0/24 || dst host 10.2.0.3) && (! dst host 10.1.0.3))"" -vn

## BERKELEY PACKET FILTERS (BPF)
- Requests a SOCK_RAW socket and setsockopt calls SO_ATTACH_FILTER
  ```
  sock = socket(PF_PACKET, SOCK_RAW, htons(ETH_P_ALL))
  ...
  setsockopt(sock, SOL_SOCKET, SO_ATTACH_FILTER, ...)
  ```
- Using BPFs with operators, bitmasking, and TCPDump creates a powerful tool for traffic filtering and parsing.
  - tcpdump {A} [B:C] {D} {E} {F} {G}
    - A = Protocol (ether | arp | ip | ip6 | icmp | tcp | udp)
    - B = Header Byte offset
    - C = optional: Byte Length. Can be 1, 2 or 4 (default 1)
    - D = optional: Bitwise mask (&)
    - E = Operator (= | == | > | < | <= | >= | != | () | << | >>)
    - F = Result of Expresion
    - G = optional: Logical Operator (&& ||) to bridge expressions
- Example:
  - tcpdump 'ether[12:2] = 0x0800 && (tcp[2:2] != 22 && tcp[2:2] != 23)'
 
## BITWISE MASKING
- To filter down to the bit(s) and not just the byte.
  - ip[0] & 0x0F > 0x05
    ip ver | IHL
    1111 | 0101
    0000 | 1111
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/1137d1e6-13c2-4a94-8698-06c3f5573c88)

## FILTER LOGIC - MOST EXCLUSIVE
- All designated bit values must be set; no others can be set
  ```
  tcp[13] = 0x11
  --or--
  tcp[13] & 0xFF = 0x11
  ```
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/ae497023-c71b-4bcc-adf4-6c9798115454)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/83db63b9-4aa0-4590-b63e-e17d88cf9c5c)

- Source Address
  - ip[12:4]& 0xFFFFFFFF = 0x0A0A0028 (most exclusive)
  - ip[12:4]& 0xFFFFFF00 = 0x0A0A0028 (Lest Exculsive, only looks at 3 octets)
- IP Header (DSCP and ECN) (MULTIPLY DSCP by 4)
  - ip[1]& 0xFC = 80
- IP Header (Flags / fragment)
  - (ip[6]& E0 = 0x20 || ip[6:2]& 0x1FFF > 0)

## FILTER LOGIC - LESS EXCLUSIVE
- All designated bits must be set; all others may be set
  - tcp[13] & 0x11 = 0x11
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/89edfd26-de54-454c-88f7-1325629c87e4)

## FILTER LOGIC - LEAST EXCLUSIVE
- At least one of the designated bits must be set to not equal 0; all others may be set
  - tcp[13] & 0x11 !=0
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/e12ec486-137f-4fb2-bac8-5cfb15eb1096)
