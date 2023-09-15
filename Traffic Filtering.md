# Traffic Filtering

- x11 filter (terminator)
```
sudo iptables -A INPUT -p tcp -m multiport  --ports 6010,6011,6012 -j ACCEPT
```

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

## NFTABLE ENHANCEMENTS
- One table command to replace:
  - iptables
  - ip6tables
  - arptables
  - ebtables
- simpler, cleaner syntax
- less code duplication resulting in faster execution
- simultaneous configuration of IPv4 and IPv6

## NFTABLES HOOKS
- ingress - netdev only
- prerouting
- input
- forward
- output
- postrouting

## INTRODUCES CHAIN-TYPES
- There are three chain types:
  - filter - to filter packets - can be used with arp, bridge, ip, ip6, and inet families
  - route - to reroute packets - can be used with ip and ipv6 families only
  - nat - used for Network Address Translation - used with ip and ip6 table families only
 
## NFTABLES SYNTAX

### 1. CREATE THE TABLE
- nft add table [family] [table]
  - [family] = ip*, ip6, inet, arp, bridge and netdev.
  - [table] = user provided name for the table.

### 2. CREATE THE BASE CHAIN
```
nft add chain [family] [table] [chain] { type [type] hook [hook]
    priority [priority] \; policy [policy] \;}
```
- * [chain] = User defined name for the chain.
- * [type] =  can be filter, route or nat.
- * [hook] = prerouting, ingress, input, forward, output or
         postrouting.
- * [priority] = user provided integer. Lower number = higher
             priority. default = 0. Use "--" before
             negative numbers.
- * ; [policy] ; = set policy for the chain. Can be
              accept (default) or drop.
- Use "\" to escape the ";" in bash

### 3. CREATE A RULE IN THE CHAIN
- nft add rule [family] [table] [chain] [matches (matches)] [statement]
  - * [matches] = typically protocol headers(i.e. ip, ip6, tcp,
            udp, icmp, ether, etc)
  - * (matches) = these are specific to the [matches] field.
  - * [statement] = action performed when packet is matched. Some
              examples are: log, accept, drop, reject,
              counter, nat (dnat, snat, masquerade)

## RULE MATCH OPTIONS
- ip [ saddr | daddr { ip | ip1-ip2 | ip/CIDR | ip1, ip2, ip3 } ]
- tcp flags { syn, ack, psh, rst, fin }
- tcp [ sport | dport { port1 | port1-port2 | port1, port2, port3 } ]
- udp [ sport| dport { port1 | port1-port2 | port1, port2, port3 } ]
- icmp [ type | code { type# | code# } ]
- ct state { new, established, related, invalid, untracked }
- iif [iface]
- oif [iface]

## MODIFY NFTABLES
- nft { list | flush } ruleset
- nft { delete | list | flush } table [family] [table]
- nft { delete | list | flush } chain [family] [table] [chain]

## MODIFY NFTABLES
- List table with handle numbers
  - nft list table [family] [table] [-a]
- Adds after position
  - nft add rule [family] [table] [chain] [position <position>] [matches] [statement]
- Inserts before position
  - nft insert rule [family] [table] [chain] [position <position>] [matches] [statement]
- Replaces rule at handle
  - nft replace rule [family] [table] [chain] [handle <handle>] [matches] [statement]
- Deletes rule at handle
  - nft delete rule [family] [table] [chain] [handle <handle>]

## MODIFY NFTABLES
- To change the current policy
  - nft add chain [family] [table] [chain] { \; policy [policy] \;}
 
# CONFIGURE IPTABLES NAT RULES

## NAT & PAT OPERATORS & CHAINS
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/b3639c13-4768-4c6f-b6c0-3c7a0b05a7b6)

## SOURCE NAT
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
- iptables -t nat -A POSTROUTING -o eth0 -s 192.168.0.1 -j SNAT --to 1.
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/bbece352-7e00-4bfc-bcca-088509874d27)
- iptables -t nat -A POSTROUTING -p tcp -o eth0 -s 192.168.0.1 -j SNAT --to 1.1.1.1:9001
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/8841634d-f51d-4de7-ac77-bcd52d5def52)

## DESTINATION NAT
- iptables -t nat -A PREROUTING -i eth0 -d 8.8.8.8 -j DNAT --to 10.0.0.1
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/8014b8d9-4fae-46cf-a8e7-87c029315443)

## DESTINATION NAT
- iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 22 -j DNAT --to 10.0.0.1:22
- iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j DNAT --to 10.0.0.2:80
- iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j DNAT --to 10.0.0.3:443
- iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080

# CONFIGURE NFTABLES NAT RULES

## CREATING NAT TABLES AND CHAINS
- Create the NAT table
  - nft add table ip NAT
- Create the NAT chains
  - nft add chain ip NAT PREROUTING { type nat hook prerouting priority 0 \; }
  - nft add chain ip NAT POSTROUTING { type nat hook postrouting priority 0 \; }

## SOURCE NAT
- nft add rule ip NAT POSTROUTING ip saddr 10.10.0.40 oif eth0 snat 144.15.60.11
- nft add rule ip NAT POSTROUTING oif eth0 masquerade

## DESTINATION NAT
- nft add rule ip NAT PREROUTING iif eth0 ip daddr 144.15.60.11 dnat 10.10.0.40
- nft add rule ip NAT PREROUTING iif eth0 tcp dport { 80, 443 } dnat 10.1.0.3
- nft add rule ip NAT PREROUTING iif eth0 tcp dport 80 redirect to 8080

# CONFIGURE IPTABLES MANGLE RULES

## MANGLE EXAMPLES WITH IPTABLES
- iptables -t mangle -A POSTROUTING -o eth0 -j TTL --ttl-set 128
- iptables -t mangle -A POSTROUTING -o eth0 -j DSCP --set-dscp 26

# CONFIGURE NFTABLES MANGLE RULES

## MANGLE EXAMPLES WITH NFTABLES
- nft add table ip MANGLE
- nft add chain ip MANGLE INPUT {type filter hook input priority 0 \; policy accept \;}
- nft add chain ip MANGLE OUTPUT {type filter hook output priority 0 \; policy accept \;}
- nft add rule ip MANGLE OUTPUT oif eth0 ip ttl set 128
- nft add rule ip MANGLE OUTPUT oif eth0 ip dscp set 26

# UNDERSTAND NETWORK BASED FILTERING

## DESCRIBE FIREWALL TYPE
- Zone-Based Policy Firewall (Zone-Policy Firewall, ZBF or ZFW)
- Host Based Firewalls
- Network Based Firewalls

## INTERPRET A DATA FLOW DIAGRAM GIVEN A SET OF FIREWALL RULES
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/c6a65922-690c-41e3-a6c8-72a3f3d3446f)

## DETERMINE POSITIONING OF FILTERING DEVICES ON A NETWORK
1. Determine network segments
2. Conduct Audit
3. Filtering devices we need
4. Device placement

## TYPICAL LOCATIONS FOR FILTERING DEVICES
- IPS
- Firewalls
- Routers
- Switches

## FILTERING DEVICE PLACEMENT EXAMPLE
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/eb2a327c-2b4a-4277-9aa0-a847caa5bcf7)

# INTERPRET CISCO ACCESS CONTROL LIST (ACL)

## ACL NUMBERING & NAMING CONVENTIONS
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/a0869e72-4589-44b2-af06-ed3717cd3d84)

## SYNTAX TO CREATE ACCESS LISTS
- Demo> enable #enter privileged exec mode
- Demo# configure terminal #enter global config mode
- Demo(config)# access-list 37 ... (output omitted) ...
- Demo(config)# ip access-list standard block_echo_request
- Demo(config)# access-list 123  ... (output omitted) ...
- Demo(config)# ip access-list extended zone_transfers

STANDARD NUMBERED ACL SYNTAX
- router(config)# access-list {1-99 | 1300-1999}  {permit|deny}  {source IP add} {source wildcard mask}
- router(config)#  access-list 10 permit host 10.0.0.1
- router(config)#  access-list 10 deny 10.0.0.0 0.255.255.255
- router(config)#  access-list 10 permit any

## STANDARD NAMED ACL SYNTAX
- router(config)# ip access-list standard [name]
- router(config-std-nacl)# {permit | deny}  {source ip add}  {source wildcard mask}
- router(config)#  ip access-list standard CCTC-STD
- router(config-std-nacl)#  permit host 10.0.0.1
- router(config-std-nacl)#  deny 10.0.0.0 0.255.255.255
- router(config-std-nacl)#  permit any

## EXTENDED NUMBERED ACL SYNTAX
- router(config)# access-list {100-199 | 2000-2699} {permit | deny} {protocol}
                {source IP add & wildcard} {operand: eq|lt|gt|neq}
                {port# |protocol} {dest IP add & wildcard} {operand: eq|lt|gt|neq}
                {port# |protocol}
- router(config)# access-list 144 permit tcp host 10.0.0.1 any eq 22
- router(config)# access-list 144 deny tcp 10.0.0.0 0.255.255.255 any eq telnet
- router(config)# access-list 144 permit icmp 10.0.0.0 0.255.255.255 192.168.0.0
                0.0.255.255 echo
- router(config)# access-list 144 deny icmp 10.0.0.0 0.255.255.255 192.168.0.0
                0.0.255.255 echo-reply
- router(config)# access-list 144 permit ip any any

## ACLS CAN BE USED FOR:
1. Filtering traffic in/out of a network interface.
2. Permit or deny traffic to/from a router VTY line.
3. Identify authorized users and traffic to perform NAT.
4. Classify traffic for Quality of Service (QoS).
5. Trigger dial-on-demand (DDR) calls.
6. Control Bandwidth.
7. Limit debug command output.
8. Restrict the content of routing updates.

## ACLS RULES
1. One ACL per interface, protocol and direction
2. Must contain one permit statement
3. Read top down
4. Inbound processed before routing
5. Outbound processed after routing
6. Does not apply for SSH or telnet traffic to device
7. Does not apply to traffic from the device
8. Only standard ACLs on VTY lines

## APPLY AN ACL TO AN INTERFACE OR LINE
- router(config)#  interface {type} {mod/slot/port}
- router(config)#  ip access-group {ACL# | name} {in | out}
- router(config)#  interface s0/0/0
- router(config-if)#  ip access-group 10 out
- router(config)#  interface g0/1/1
- router(config-if)#  ip access-group CCTC-EXT in
- router(config)#  line vty 0 15
- router(config)#  access-class CCTC-STD in

# ACL PLACEMENT

## ACL PLACEMENT 1
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/789a2f10-3283-439f-a284-cbcbc9310cf1)
- Where would you place a Standard ACL (Router and Interface) to block traffic from host 10.3.0.4 to host 10.5.0.7? Would it be in or outbound?

## ACL PLACEMENT 2
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/38e7ff3b-8acb-4186-82b7-43118f609695)
- Where would you place an Extended ACL to block traffic from host 10.1.0.1 to 10.5.0.17? Would it be in or outbound?

## ACL PLACEMENT 3
- Interpret this ACL:
  - ip access-list 101 deny udp host 19.3.0.29 10.5.0.0 0.0.0.255 eq 69
  - ip access-list 101 deny tcp any 10.3.0.0 0.0.0.255 eq 22
  - ip access-list 101 deny tcp any 10.1.0.0 0.0.0.255 eq 23
  - ip access-list 101 deny icmp any 10.5.0.0 0.0.0.255 echo
  - ip access-list 101 deny icmp any 10.5.0.0 0.0.0.255 echo-reply
- What Type of list is this?
- What would it do?
- Where should it be placed (use diagram on previous slide)?
- What direction?

