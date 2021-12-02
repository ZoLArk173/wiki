# Chapter 3 Transport Layer

## Transport services and protocols

- provide **logical communication** between app processes running on different hosts
- transport protocols run in end systems
  - send side
    - breaks app messages in segments, passes to network layer
  - receive side
    - reassembles segments into messages, passes to app layer
- more than one transport protocol available to apps
  - Internet: TCP and UDP

### Transport vs Network Layer

- network layer
  - logical communication between hosts
- transport layer
  - logical communication between processes
    - relies on network layer services

### Internet Transport-Layer Protocols

- TCP
  - congestion control
  - flow control
  - connection setup
- UDP
- not available:
  - delay guarantees
  - bandwidth guarantees

## Multiplexing / Demultiplexing

- multiplexing at sender
  - handle data from multiple sockets
  - add transport header (later used for demultiplexing)
- demultiplexing at receiver
  - use header info to deliver received segments to correct socket

### How demultiplexing works?

- host receives IP datagrams
  - each **datagram** has source IP address, destination IP address
  - each **datagram** carries one transport-layer **segment**
  - each **segment** has source, destination port number
- host uses **IP address & port numbers** to direct segment to appropriate socket

### Connectionless Demultiplexing

when host receives UDP segment:

- checks destination port number in segment
- directs UDP segment to socket with that port number

IP datagrams with **same destination port number**, but different source IP addresses or source port will be directed to **same socket at destination**.

### Connection-Oriented Demultiplexing

- TCP socket identified by 4-tuple
  - source IP address
  - source port number
  - destination IP address
  - destination port number
- receiver **uses all four values** to direct segment to appropriate socket
- server host may support many simultaneous TCP sockets
  - each socket **identified by its own 4 tuple**
- web servers have different sockets for each connecting client
  - non-persistent HTTP will have different socket for each request 

## Connectionless Transport: UDP

User Datagram Protocol

- "no frills", "bare bones" Internet transport protocol
- "best effort" service, UDP segments may be:
  - lost
  - delivered out-of-order to app
- **connectionless**:
  - no handshaking between UDP sender, receiver
  - each UDP segment handled independently of others
- usage:
  - streaming multimedia apps (loss tolerant, rate sensitive)
  - DNS
  - SNMP
- reliable transfer over UDP
  - add reliability at application layer
  - application-specific error recovery

### Segment Header



