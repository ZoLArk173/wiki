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
