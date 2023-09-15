# Traffic Filtering

## WHY FILTER TRAFFIC?
- Block malicious traffic
- Decrease load on network infrastructure
- Ensure data flows in an efficient manner
- Ensure data gets to intended recipients and only intended recipients
- Obfuscate network internals

## PRACTICAL APPLICATIONS FOR FILTERING
- Network Traffic - allow or block traffic to/from remote locations.
- Email addresses - to block unwanted email to reduce risk or increase productivity
- Computer applications in an organization environment - for security from vulnerable software
- MAC filtering - also for security to allow only specific computers access to a network

# NETWORK TRAFFIC FILTERING CONCEPTS

## DEFAULT POLICIES
- Explicit - precisely and clearly expressed
- Implicit - implied or understood

## BLOCK-LISTING VS ALLOW-LISTING
- Block-Listing (Formerly Black-List)
  - Implicit ACCEPT
  - Explicit DENY
- Allow-Listing (Formerly White-List)
  - Implicit DENY
  - Explicit ACCEPT

## DISCUSS FILTERING DEVICE TYPES
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/beb7b01b-5caa-4673-b04e-032faeaf59ad)

## OPERATION MODES
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/5e219f74-c980-4f86-8801-0138993ac550)

## FIREWALL FILTERING METHODS
- Stateless (Packet) Filtering (L3+4)
- Stateful Inspection (L4) (TCP or UDP)
- Circuit-Level (L5)
- Application Layer (L7)
- Next Generation (NGFW) (L7)

## SOFTWARE VS HARDWARE VS CLOUD FIREWALLS
- Software - typically host-based
- Hardware - typically network-based
- Cloud - provided as a service

## TRAFFIC DIRECTIONS
- A to B
  - Traffic originating from the localhost to the remote-host
    - You (the client) are the client sending traffic to the server.
  - Return traffic from that remote-host back to the localhost.
    - The server is responding back to you (the client).
- B to A
  - Traffic originating from the remote-host to the localhost.
    - A client is trying to connect to you (the server)
  - Return traffic from the localhost back to the remote-host.
    - You (the server) are responding back to the client.
   
## BASIC FILTERING INTENT
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/c0310bfa-6f7b-4ff4-9cf8-5a96c2823412)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/3a503f99-5aeb-42f9-9c3f-1c4cf02eeacd)

# UNDERSTAND HOST BASED FILTERING

## WINDOWS, LINUX, OR MAC
- Windows - Norton, Mcafee, ZoneAlarm, Avast, etc.
- Linux - iptables, nftables, pfSense, OPNsense, etc.
- MAC - Little Snitch, LuLu, Vallum, etc.

## NETFILTER FRAMEWORK
- Made to provide:
  - packet filtering
  - stateless/stateful Firewalls
  - network address and port translation (NAT and PAT)
  - other packet manipulation
 
## NETFILTER HOOKS - > CHAIN
- NF_IP_PRE_ROUTING → PREROUTING
- NF_IP_LOCAL_IN → INPUT
- NF_IP_FORWARD → FORWARD
- NF_IP_LOCAL_OUT → OUTPUT
- NF_IP_POST_ROUTING → POSTROUTING

## NETFILTER PARADIGM
1. tables - contain chains
2. chains - contain rules
3. rules - dictate what to match and what actions to perform on packets when packets match a rule

## SEPARATE APPLICATIONS
- Netfilter created several (separate) applications to filter on different layer 2 or layer 3+ protocols.
  - iptables - IPv4 packet administration
  - ip6tables - IPv6 packet administration
  - ebtables - Ethernet Bridge frame table administration
  - arptables - arp packet administration

 # CONFIGURE IPTABLES FILTERING RULES

## TABLES OF IPTABLES
- filter - default table. Provides packet filtering.
- nat - used to translate private ←→ public address and ports.
- mangle - provides special packet alteration. Can modify various fields header fields.
- raw - used to configure exemptions from connection tracking.
- security - used for Mandatory Access Control (MAC) networking rules.

## CHAINS OF IPTABLES
- PREROUTING - packets entering NIC before routing
- INPUT - packets to localhost after routing
- FORWARD - packets routed from one NIC to another. (needs to be enabled)
- OUTPUT - packets from localhost to be routed
- POSTROUTING - packets leaving system after routing

## CHAIN FLOW
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/98ebfd57-8694-453d-a645-602865abad11)

## CHAINS ASSIGNED TO EACH TABLE
- filter - INPUT, FORWARD, and OUTPUT
- nat - PREROUTING, POSTROUTING, INPUT, and OUTPUT
- mangle - All chains
- raw - PREROUTING and OUTPUT
- security - INPUT, FORWARD, and OUTPUT

## COMMON IPTABLE OPTIONS
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/5fce9a59-56a0-47c4-8acd-6891aff2a0dc)

## IPTABLES SYNTAX
```
iptables -t [table] -A [chain] [rules] -j [action]
```
- Table: filter*, nat, mangle
- Chain: INPUT, OUTPUT, PREROUTING, POSTROUTING, FORWARD

## IPTABLES RULES SYNTAX
- -i [ iface ]
- -o [ iface ]
- -s [ ip.add | network/CIDR ]
- -d [ ip.add | network/CIDR ]

## IPTABLES RULES SYNTAX
- -p icmp [ --icmp-type { type# | code# } ]
- -p tcp [ --sport | --dport { port1 |  port1:port2 } ]
- -p tcp [ --tcp-flags { SYN | ACK | PSH | RST | FIN | URG | ALL | NONE } ]
- -p udp [ --sport | --dport { port1 | port1:port2 } ]

## IPTABLES RULES SYNTAX
- -m to enable iptables extensions:
  - -m state --state [ NEW | ESTABLISHED | RELATED | UNTRACKED | INVALID ]
  - -m mac [ --mac-source | --mac-destination ] [mac]
  - -p [tcp|udp] -m multiport [ --dports | --sports | --ports { port1 | port1:port15 } ]
  - -m bpf --bytecode [ 'bytecode' ]
  - -m iprange [ --src-range | --dst-range { ip1-ip2 } ]

## IPTABLES ACTION SYNTAX
- ACCEPT - Allow the packet
- REJECT - Deny the packet (send an ICMP reponse)
- DROP - Deny the packet (send no response)
```
-j [ ACCEPT | REJECT | DROP ]
```

## MODIFY IPTABLES
- Flush table
  - iptables -t [table] -F
- Change default policy
  - iptables -t [table] -P [chain] [action]
- Lists rules with rule numbers
  - iptables -t [table] -L --line-numbers
- Lists rules as commands interpreted by the system
  - iptables -t [table] -S
- Inserts rule before Rule number
  - iptables -t [table] -I [chain] [rule num] [rules] -j [action]
- Replaces rule at number
  - iptables -t [table] -R [chain] [rule num] [rules] -j [action]
- Deletes rule at number
  - iptables -t [table] -D [chain] [rule num]
 
# CONFIGURE NFTABLES FILTERING RULES

## NFTABLE FAMILIES
- ip - IPv4 packets
- ip6 - IPv6 packets
- inet - IPv4 and IPv6 packets
- arp - layer 2
- bridge - processing traffic/packets traversing bridges.
- netdev - allows for user classification of packets - nftables passes up to the networking stack (no counterpart in iptables)
