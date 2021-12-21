# Chapter 4 Network Layer

## Network Layer

- transport segment form sending to receiving host
- on sending side, encapsulates (encode) segments into datagrams
- on receiving side, delivers segment to transport layer
- router examines header fields in all IP datagrams passing though it

### Network-Layer Functions

- forwarding
  - move packets from router's input to appropriate router output
- routing
  - determine route taken by packets from source to destination
  - routing algorithms

### Data Plane

- local, per-router function
- determines how datagram arriving on router input port is forwarded to router output port
- forwarding function

### Control Plane

- network-wide logic
- determines how datagram is routed among routers along end-end path from source to destination
- approaches:
  - traditional routing algorithms
    - implemented in routers
  - software-defined networking
    - implemented in (remote) servers

#### Per-Router Control Plane

individual routing algorithm components in each and every router interact in the control plane

#### Logical Centralized Control Plane

A distinct (typically remote) controller interacts with local control agents (CAs)

#### Network Service Model

services for individual datagrams

- guaranteed delivery
- guaranteed delivery with less than 40 msec delay

services for a flow of datagrams

- in-order datagram delivery
- guaranteed minimum bandwidth to flow
- restrictions on changes in inter-packet spacing

## What's Inside a Router

high-level view of generic router architecture:

- routing processor
  - routing, management control plane(software)
  - operates in millisecond time frame
- high-speed switching fabric
  - forwarding data plane (hardware)
  - operates in nanosecond time frame

Input port functions

- line termination
  physical layer
  - bit-level reception
- link layer protocol (receive)
  data link layer
  - Ethernet
- lookup, forwarding, queuing
  decentralized switching
  - using header field values, lookup output port using forwarding table in input port memory (match and action)
  - complete input port processing at line speed
  - queuing: if datagram arrive faster than forwarding rate into switch fabric
  - destination-based forwarding
    - forward based only on destination IP address
  - generalized forwarding
    - forward based on any set of header field values

### Destination-Based Forwarding

