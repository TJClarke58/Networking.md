# Day 5 NETWORK RECONNAISSANCE

## RECONNAISSANCE
- Active - comprehensive report of possible open or closed ports at the time of the scan
  - External (Ping scans, NMAP scans, Port Scans, OS Identification)
  - Internal (DNS Queries, ARP Requests, Ping Scans, Port Scans, Network Scanning)
- Passive - identifies network services by observing traffic generated by servers and hosts as it passes an observation point
  - External (DNS Lookups (DIG), Whois, Job site listings, Shodan)
  - Internal (Packet Sniffing, ARP Cache, IP Address, Running Services, Open Ports)
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/cda901ba-6e8b-4a9a-9792-1e519f751f88)

## PASSIVE RECONNAISSANCE
- Gathering information about targets without direct interaction
- Not as straight forward and requires more time than active reconnaissance
- Lower risk of discovery
- Involves identifying:
  - IP addresses and Sub-Domains
  - External and 3rd party sites
  - People and Technologies
  - Content of Interest
  - Vulnerabilities
- Possible tools for gathering:
  - WHOIS queries
  - Job site listings
  - Phone Numbers
  - Google searches
  - Passive OS fingerprinting
 
## PASSIVE EXTERNAL NETWORK RECONNAISSANCE
- Information gathered outside of the network using passive methods
- Allows for more efficient attacks and plans

## PASSIVE EXTERNAL NETWORK RECONNAISSANCE: DNS
- Resolves hostnames to IP addresses
- RFC 3912
- WHOIS queries

## PASSIVE EXTERNAL NETWORK RECONNAISSANCE: DIG
- Typically between primary and secondary DNS servers
- If allowed to transfer externally hostnames, IPs, and IP blocks can be determined
```
instructor@net1:~$ dig ccboe.net
instructor@net1:~$ dig ccboe.net MX
instructor@net1:~$ dig ccboe.net SOA
instructor@net1:~$ dig ccboe.net TXT
```

## PASSIVE EXTERNAL NETWORK RECONNAISSANCE: ZONE TRANSFERS
- Returns DNS information
- Supplements base queries
```
instructor@net1:~$ dig axfr @nsztm1.digi.ninja zonetransfer.me
```

## PASSIVE EXTERNAL NETWORK RECONNAISSANCE: HOST HISTORY
- netcraft
- wayback machine

## PASSIVE EXTERNAL NETWORK RECONNAISSANCE: GOOGLE SEARCHES
- subdomains
- technologies
```
*ccboe.net -site:*.ccboe.net
```
```
site:*.ccboe.net "Powered by"
```

## PASSIVE EXTERNAL NETWORK RECONNAISSANCE: SHODAN
- Reveals information about technologies, remote access services, improperly configured services, and network infrastructure.
- When selected can give additional information and applicable vulnerabilities
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/42574f70-3175-474a-9963-fd875096b3fe)

## NETWORK SCANNING
- Scanning Strategy
  - Remote to Local
  - Local to Remote
  - Local to Local
  - Remote to Remote
- Scanning Approach
  - Aim
    - Wide range target scan
    - Target specific scan
  - Method
    - Single source scan - operates from a one to many fashion
    - Distributed scan - multiple systems in a union to scan a network or host of interest
- Broadcast Ping and Ping sweep
  - Broadcast - broadcast ping sends an ICMP echo request to the network broadcast address.
  - Ping Sweep - sends an ICMP echo request to every usable address on a network.
  ```
  fping -g -a 10.1.0.0/24
  nmap –sn 10.0.0.0/24 (was -sP which is now deprecated)
  for i in {1..254}; do ping -c 1 -W 1 10.1.1.$i | grep 'from'; done
  ```
  
- ARP scan
  ```
  sudo arp-scan --interface=eth0 --localnet
  sudo arp-scan --interface=eth0 10.1.0.0/24
  arping –c 1 –i eth0 172.16.32.2 (can only scan a single host, results vary)
  for ip in $(sew 1 254) ; do if ping -c 1 10.1.0.$ip>/dev/null; then echo "10.1.0.$ip UP"; fi ;done
  nmap –PR 172.16.32.2(legitimate scan that often does not show results)
  nmap -PR -6 fe80::f816:3eff:fed9:5116/64(takes a long time to run)
  ```
  
- SYN scan
  ```
  nmap –sS 172.16.32.2
  hping3 172.16.32.2 -S -V -p 443
  ```
  
- Full connect scan
  ```
  nmap –sT –sV 10.16.32.23
  (Full TCP connect, service versioning)
  nmap –sT 172.16.32.2
  ```
- Null scan
  ```
  nmap -sN 10.50.1.1
  hping3 -c 1 -V -p 80 -s 5050 -Y 10.50.1.1
  ```
  
- FIN scan
  ```
  nmap -sF 25.50.75.100
  hping3 -c 1 -V -p 80 -s 5050 -F 25.50.75.100
  ```
  
- XMAS tree scan
  ```
  nmap -sX 7.92.5.19
  hping3 -c 1 -V -p 80 -s 5050 -M 0 -UPF 7.92.5.19
  ```
  
- UDP scan
  ```
  namp -sU -v 10.10.100.3
  ```
  
- Idle scan
  ```
  nmap -sI 10.10.5.6 25.23.4.7
  ```
  
- ACK/Window scan
  ```
  nmap -sW 10.66.35.10
  ```
  
- RPC scan
  ```
  nmap -sR 10.50.22.29
  ```
  
- FTP scan
  ```
  nmap -b 10.1.1.3 10.2.5.1
  ```
  
- decoy scan
  ```
  nmap -D 1.2.3.4, 5.6.7.8,ME 100.200.10.20
  ```
- OS fingerprinting scan
  ```
  nmap -O 6.2.9.5
  ```
  
- version scan
  ```
  nmap -sV 10.30.50.70
  ```
  
- Maimon scan
  ```
  nmap -sM 10.90.20.80
  ```
  
- Protocol ping
  ```
  nmap -PO 43.76.12.93
  ```
  
- Discovery probes
  - ICMP (nmap -PE 88.55.22.77)
  - Timestamp (nmap -PP 10.9.8.7)
  - Netmask (nmap -PM -Pn 5.3.7.9)
  - TCP SYN (nmap -PS21-50 55.66.77.22)
  - TCP ACK (nmap -PA21-50 1.9.2.8 or hping3 -c 1 -V -p 80 -s 5050 -A 10.9.2.8 (TCP ACK Scan)
  - UDP (nmap -PU21-50 45.60.75.90)
    
- SCTP INIT scan
  ```
  nmap -sY 17.34.51.68
  ```

## NETWORK SCANNING - CODE
```
nmap [Options] [Target IP/ Subnet]
nc [Options] [Target IP] [Target Port]
```

## NETWORK MAPPING
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/9be8d089-e370-4acb-9840-a7038bdfd68b)

## Situational Awareness
- Where am I? (uname -n/hostname)
- Who Am I? (whoami/id)
- Who else is on here? (who/w)
- What am I allowed to do? (sudo -l)
- What ports are open? (netstat -anlptu/ss -nlptu) RUN SUDO
- Are there any special services running? (ps -elf)
- What interfaces do I have? (ifconfig/ip addr (ip a))
- What other hosts does this box know? (arp -a/ip n)
- What routes/networks does this host know? (route/ip r)

# Vyos router
```
ssh vyos@172.16.20.1
```
password: password

- Similar command shown above
- Different commands
  - show int
  - show config
 
# NMAP syntax
```
nmap 172.16.82.106 -Pn -p 3000-3999 -T4 --min-rate 10000 -A -vvvv
```
```
nmap 172.16.82.1-10  -p 1-1000,2552  -T4 --min-rate 10000 -A -vvvv -Pn
```
```
nmap 172.16.82.1/29  -p-  -T4 --min-rate 10000 -A -vvvv -Pn
```
