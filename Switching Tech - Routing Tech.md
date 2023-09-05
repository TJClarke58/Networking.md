# LAYER 2 SWITCHING TECHNOLOGIES

## SWITCH OPERATION
- Fast Forward - Only Destination MAC
- Fragment Free - First 64 bytes
- Store and Forward - Entire Frame and FCS

## CAM Table
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/2d459b3a-933d-468b-97d2-07daf22bb819)
- Learn - Examining the Source MAC Address
- Forward - Examining the Destination MAC Address

## VLANS AND IEEE 802.1Q
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/82ee03d9-e37a-406e-ba43-b51fd745cb18)

## IEEE 802.1AD "Q-IN-Q"
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/8e2f29c7-f0b0-46e0-a009-e402c72f5f87)

## SPANNING TREE PROTOCOL (STP)
- Root decision process
  1. Elect root Bridge
  2. Identify the Root ports on non-root bridge
  3. Identify the Designated port for each segmentRIP

Hop count

EIGRP

Bandwidth, Delay, Load, Reliability

OSPF

Cost (Bandwidth)

IS-IS

Cost (Assigned by Admin)

BGP

Policy assigned my Admin
  4. Set alternate ports to blocking state
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/16830423-faf8-4ee3-ba3f-7434fba5903f)

## LAYER 2 DISCOVERY PROTOCOLS
- Cisco Discovery Protocol (CDP) = used to share information with other directly connected Cisco devices
- Foundry Discovery Protocol (FDP)
- Link Layer Discovery Protocol(LLDP) = vendor-neutral neighbor discovery protocol similar to CDP

## DTP (DYNAMIC TRUNKING PROTOCOL)
- Server - can create, modify or delete VLANs. Can create and forward VTP messages.
- Client - can only adopt VLAN information in VTP messages. Can forward VTP messages.
- Transparent - only forwards VTP messages but does not adopt any of the information.
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/7b2ae681-77bc-43bd-80d4-9d59fff35ac7)
- VTP advertisements are sent over all trunk links. VTP messages advertise the following on its trunk ports:
  - Management domain
  - Configuration revision number
  - Known VLANs and their specific parameters
- There are three versions of VTP, namely version 1, version 2

## VTP (VLAN TRUNKING PROTOCOL)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/1e088136-b55d-4b35-a3b9-ca22d0490280)

## PORT SECURITY
- Modes
  - shutdown
  - restrict
  - protect
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/80972f2e-906b-40c8-a986-4c84ecc46b00)

# LAYER 3 ROUTING TECHNOLOGIES
- ROUTING
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/b9c610ad-437f-444b-9902-2d04ce4cfc27)
- Routing Table
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/2162de72-4aa2-4db1-9a27-e52aa11705c3)
- ANATOMY
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/192b0082-f337-422f-88ce-45678e08e000)
  - Ultimate route is any routing table entry that has a next-hop IPv4 address, exit interface, or both.
  - Level 1 route is any route with the subnet mask (CIDR) is equal to or less than the classful mask of the network address. A level 1 route can be a:
    - Network route - A network route that has a subnet mask equal to that of the classful mask.
      - Class A - 255.0.0.0 (/8)
      - Class B - 255.255.0.0 (/16)
      - Class C - 255.255.255.0 (/24)
    - Supernet route - A network route with a mask less (smaller) than the classful mask.
    - Default route - A default route is a static route with the address 0.0.0.0/0.
  - Parent route is a level 1 network that is subnetted. A parent route will never be an ultimate route.
  - Level 2 child route are the subnets of a classful network address.

## LOOKUP PROCESS 
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/c3a3bc62-5ec0-4a7b-865a-657460cb67db)

## ROUTED VS ROUTING
- Routed protocol is a protocol that allows data to be routed
- Routing Protocol are used by routers communicate routing information with each other
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/fc6c4ad0-b4a2-4caa-9d90-82937a161ba7)

## FIRST HOP REDUNDANCY PROTOCOLS
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/0332b740-3150-4c0f-a1b1-0a3fe1b16985)

## ADMINISTRATIVE DISTANCE 
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/625375e1-ebbc-4fbc-a8f0-132746b56f23)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/da305137-12de-401d-a72f-749d5904b49b)
- RIP = Hop count
- EIGRP = Bandwidth, Delay, Load, Reliability
- OSPF = Cost (Bandwidth)
- IS-IS = Cost (Assigned by Admin)
- BGP = Policy assigned my Admin

- Some of the most common metrics that routing protocols can use are:
  - hop
  - bandwidth
  - delay
  - reliability
  - load
  - MTU
  - cost
  - administratively defined
 
## STATIC ROUTING
- Static routes are manually configured on each router by a network administrator to route traffic for every specific remote network.
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/5d6f91f4-cd8a-4dab-86d8-3ffad4f9a50f)

## STATIC ROUTING ADVANTAGES AND DISADVANTAGES
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/870c8e9f-8fff-40aa-a6e1-e56c8af88891)

## DYNAMIC ROUTING
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/8288f20b-ac64-49d1-8865-da09056af1ba)
- The purpose of dynamic routing protocols includes:
  - Discover new remote networks
  - Maintaining current routing information
  - Choose best path to remote networks
  - Recalculate a new patch to a remote network should the primary fail

## DYNAMIC ROUTING ADVANTAGES AND DISADVANTAGES
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/edf8c5c6-ad3e-4dd3-a0e6-2662ae1201dd)

## ROUTING PROTOCOL COMPARISON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/7c07169b-5303-445c-ac59-75569dc25418)

## IGP vs EGP
- Interior Gateway Protocols (IGP): Routing protocols that are used within an AS. Referred to as intra-AS routing. Organizations and service providers IGPs on their internal networks. IGPs include RIP, EIGRP, OSPF, and IS-IS.
- Exterior Gateway Protocols (EGP): Used primarily for routing between autonomous systems. Referred to as inter-AS routing. Service providers and large companies will interconnect their AS using an EGP. The Border Gateway Protocol (BGP) is the only currently viable EGP and is the official routing protocol used by the Internet.
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/706fc1aa-158c-4975-923d-2b52ef99b91c)

## CONTROLLING ENTITIES
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/bd023180-8ab5-4317-8d44-53442ae202a9)

## DISTANCE VECTOR
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/a843164c-51c5-4f4b-a54d-3ac976b3ddf1)
- Distance Vector protocols are simplistic in their operation. They share entire routing tables with their directly connected neighbors and from these shared tables they determine two factors:
  - Distance: This identifies how far away the destination network is from the router and is based on a metric such as the hop count, cost, bandwidth, delay, and more. It takes the learned distance from their neighbor, adds the distance to their neighbor, and this gives them a total distance.
  - Vector: This specifies the direction to the remote network. The router that advertised the route to the router is the one the router will need to send traffic to that will get the traffic to the remote network and the interface it was learned on.
- There are four distance vector IPv4 IGPs:
  - RIPv1: First generation legacy protocol
  - RIPv2: Simple distance vector routing protocol
  - IGRP: First generation Cisco proprietary protocol (obsolete and replaced by EIGRP)
  - EIGRP: Advanced version of distance vector routing

## LINK STATE
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/6a31148e-72e1-4aa2-8449-124377e13887)
- Link-state protocols work best in situations where:
  - The network design is hierarchical, usually occurring in large networks
  - Fast convergence of the network is crucial
  - The administrators have good knowledge of the implemented link-state routing protocol
- There are two link-state IPv4 IGPs:
  - OSPF: Popular open standards-based routing protocol
  - IS-IS: Popular in service provider networks

## BGP
- Road-map of the Internet
- Routes traffic between Autonomous System (AS) Number
- Advertises IP CIDR address blocks
- Establishes Peer relationships
- Complicated configuration
- Complicated and slow path selection

## BGP OPERATION
- How BGP chooses the best path:
  - Advertise a more specific route. 192.168.1.0 /24 is more specific than 192.168.0.0 /16.
  - Offer a shorter route to certain blocks of IP addresses. A route to ip prefix with 4 AS 'hops" is better than route with 5 AS 'hops'

## BGP HIJACKING
- Illegitimate advertising of addresses
- BGP propagates false information
- Purpose:
  - stealing prefixes
  - monitoring traffic
  - intercept (and possibly modify) Internet traffic
  - 'black holing' traffic
  - perform MitM

## BGP HIJACKING DEFENSE
- IP prefix filtering
- BGP hijacking detection
  - Tracking the change in TTL of incoming packets
  - Increased Round Trip Time (RTT) which increases latency
  - Monitoring misdirected traffic (change in AS path from tools like Looking Glass)
- BGPSec

