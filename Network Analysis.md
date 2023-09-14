# Netwok Analysis

## FINGERPRINTING AND HOST IDENTIFICATION
- Variances in the RFC implementation for different OS’s and systems enables the capability for fingerprinting
- Tools used for fingerprinting and host identification can be used passively(sniffing/fingerprinting) or actively(scanning)

## P0F (PASSIVE OS FINGERPRINTING)
- Looks at variations in initial TTL, fragmentation flag, default IP header packet length, window size, and TCP options
- Configuration stored in:
  - /etc/p0f/p0f.fp
- P0f Signature Database
  - more /etc/p0f/p0f.fp

## NETWORK TRAFFIC SNIFFING
- What makes traffic capture possible?
  - Libpcap
  - WinPcap (outdated)
  - NPCAP

## NETWORK TRAFFIC BASELINING
- Snapshot of what the network looks like during a time frame
- No industry standard
- 7 days to establish the initial snapshot
- Prerequisite Information

## NETWORK BASELINE OBJECTIVE
- Determines the current state of your network
- Ascertain the current utilization of network resources
- Identify normal vs peak network traffic time frames
- Verify port / protocol usage

## ANALYZE NETWORK TRAFFIC STATISTICS
- Protocol Hierarchy
- Conversations
- Endpoints
- I/O Graph
- IPv4 and IPv6 Statistics
- Expert Information
```
sudo tcpdump -r analysis-demo.pcap 'tcp[13] = 0x02' | awk '{print $3}' | cut -d. -f1,2,3,4 | sort | uniq -c
```

## NETWORK DATA TYPES
- Full Packet Capture Data
- Session Data
  - sflow
  - NetFlow
- Statistical Data
- Packet String Data
- Alert Data
- Log Data

## DATA COLLECTION DEVICES
- Sensors
  - In-Line
  - Passive
 
## METHODS OF DATA COLLECTION
- TAP
- SPAN
- ARP Spoofing (MitM)

## ANOMALY DETECTION
- Indicator of Attack (IOA)
  - Proactive
  - A series of actions that are suspicious together
  - Focus on Intent
  - Looks for what must happen
    - Code execution. persistence, lateral movement, etc.

## ANOMALY DETECTION
- Indicator of Compromise (IOC)
  - Reactive
  - Forensic Evidence
  - Provides Information that can change
    - Malware, IP addresses, exploits, signatures

## INDICATORS
- .exe/executable files
- NOP sled
- Repeated Letters
- Well Known Signatures
- Mismatched Protocols
- Unusual traffic
- Large amounts of traffic/ unusual times

## POTENTIAL INDICATORS OF ATTACK
- Destinations
- Ports
- Public Servers/DMZs
- Off-Hours
- Network Scans
- Alarm Events
- Malware Reinfection
- Remote logins
- High levels of email protocols

## POTENTIAL INDICATORS OF COMPROMISE
- Unusual traffic outbound
- Anomalous user login or account use
- Size of responses for HTML
- High number of requests for the same files
- Using non-standard ports/ application-port mismatch
- Writing changes to the registry/system files
- DNS requests
- Unexpected/ unusual patching
- Unusual tasks

## TYPES OF MALWARE
- Adware/Spyware
  - large amounts of traffic/ unusual traffic
  - IOA
    - Destinations
  - IOC
    - Unusual traffic outbound
- Virus
  - phishing/ watering hole
  - IOA
    - Alarm Events, Email protocols
- Worm
  - phishing/ watering hole
  - IOA
    - Alarm events
  - IOC
    - changes to registry/ system files
- Trojan
  - beaconing
  - IOA
    - Destinations
  - IOC
    - Unusual traffic outbound, unusual tasks, changes to registry/ system files
- Rootkit
  - IOA
    - Malware reinfection
  - IOC
    - Anomalous user login/ account use
- Backdoor
  - IOA
    - Remote logins
  - IOC
    - Anomalous user login/ account use
- Botnets
  - large amounts of IPs
  - IOA
    - Destinations, remote logins
  - IOC
    - Unusual tasks, anomalous user login/ account use
- Polymorphic and Metamorphic Malware
  - Depends on the malware type/class
- Ransomware
  - IOA
    - Destinations, Ports, Malware reinfection
  - IOC
    - Unusual traffic outbound, non-standard ports, unusual tasks
- Mobile Code
  - IOA
    - Depends on the malware type/class
- Information-Stealing Worms
  - phishing/ watering hole, large amounts of traffic/ unusual traffic
  - IOA
    - Alarm events, Destinations
  - IOC
    - changes to registry/ system files, Unusual traffic outbound
- BIOS/ Firmware Malware
  - IOA
    - Malware reinfection

## POTENTIAL METHODS OF DETECTION FOR IOAS AND IOCS
- Display Filters
- Follow Streams
- BPFs
- Color Coding
- Hex Outputs

## DECODING
- enca
- chardet
- iconv

