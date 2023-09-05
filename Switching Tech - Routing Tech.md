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
  3. Identify the Designated port for each segment
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
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/da305137-12de-401d-a72f-749d5904b49b)
- RIP = Hop count
- EIGRP = Bandwidth, Delay, Load, Reliability
- OSPF = Cost (Bandwidth)
- IS-IS = Cost (Assigned by Admin)
- BGP = Policy assigned my Admin
