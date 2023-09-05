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
  - 32 = word
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
