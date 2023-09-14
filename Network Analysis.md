# Netwok Analysis

## FINGERPRINTING AND HOST IDENTIFICATION
- Variances in the RFC implementation for different OS’s and systems enables the capability for fingerprinting
- Tools used for fingerprinting and host identification can be used passively(sniffing/fingerprinting) or actively(scanning)

## P0F (PASSIVE OS FINGERPRINTING)
- Looks at variations in initial TTL, fragmentation flag, default IP header packet length, window size, and TCP options
- Configuration stored in:
  - /etc/p0f/p0f.fp
