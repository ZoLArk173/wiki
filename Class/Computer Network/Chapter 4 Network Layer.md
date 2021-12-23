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

forwarding table to record the range of IP of every link

but it cannot deal with ranges that are not divided up so nicely

### Longest Prefix Matching

When looking for forwarding table entry for given destination address, use **longest** address prefix that matches destination address.

Often performed using ternary content addressable memories (TCAMs)

- content addressable
  - present address to TCAM, retrieve address in one clock cycle, regardless of table size

### Switching Fabrics

- transfer packet from input buffer to appropriate output buffer
- switching rate: rate at which packets can be transfer from inputs to outputs
  - often measured as multiple of input/output line rate
  - N inputs: switching rate N times line rate desirable
- three types of switching fabrics
  - memory
  - bus
  - crossbar

#### Switching via Memory

- traditional computers with switching under direct control of CPU
- packet copied to system's memory
- speed limited by memory bandwidth

#### Switching via a Bus

- datagram form input port memory to output port memory via a shared bus
- switching speed limited by bus bandwidth

#### Switching via Interconnection Network

- overcome bus bandwidth limitations
- fragmenting datagram into fixed length cells, switch cells though the fabric

### Input Port Queuing

- fabric slower than input ports combined, queuing may occur at input queues
  - queueing delay and loss due to input buffer overflow
- Head-of-the-Line blocking (HOL)
  - queued datagram at front of queue prevents others in queue from moving forward

### Output Ports

- buffering required when datagrams arrive for fabric faster than the transmission rate
  - datagram can be lost due to congestion, lack of buffers
- scheduling discipline chooses among datagrams
  - priority scheduling: who gets best performence, network neutrality

### Output Port Queueing

- buffering when arrival rate via switch exceeds output line speed
- queueing delay and loss due to output port buffer overflow

### Buffering

- average buffering equal to RTT times link capacity $C$
  RTT = 250msec, C = 10Gbps:
  $$
  RTT * C= 2.5Gbit
  $$

- recent recommendation:
  with $N$ flows, buffering equal to:
  $$
  \frac{RTT*C}{\sqrt{N}}
  $$

### Scheduling Mechanisms

choose next packet to send on link

#### FIFO Scheduling

send in order of arrival to queue

discard policy: if packet arrives to full queue

- tail drop
  - drop arriving packet
- priority
  - drop on priority basis
- random
  - drop randomly

#### Priority Scheduling

send highest priority queued packet

multiple classes, with different priorities

- class may depend on making or other header info
  - IP source or destination
  - port numbers

##### Round Robin (RR) scheduling

- multiple classes
- cyclically scan class queues, sending one complete packet from each class

##### Weighted Fair Queuing (WFQ)

- generalized Round Robin
- each class gets weighted amount of service in each cycle

## IP: Internet Protocol

- addressing conventions
- datagram format
- packet handling conventions

### IP Datagram Format

- IP protocol version
- header length (bytes)
- type of data
- total datagram length (bytes)
- 16-bit identifier, fragment flag, fragment offset
  - for fragmentation/reassembly
- time to live
  - max number remaining hops (decremented at each router)
- upper layer
  - upper layer protocol to deliver payyload to
- header checksum
- source and destination IP address
- data (TCP or UDP segment)

overhead: 20 bytes of TCP + 20 bytes of IP = 40 bytes + application layer overhead

### IP Fragmentation, Reassembly

- network links have MTU (max transfer size), largest possible link-level frame
  - different link types have different MTUs
- large IP datagram divided (fragmented) within network
  - one datagram becomes several datagrams
  - reassembled only at final destination
  - IP header bits used to identify, order related fragments

### IPv4 Addressing

- IP address: 32-bit identifier for host, router interface
- interface: connection between host/router and physical link
  - router's typically have multiple interfaces
  - host typically has one or two interfaces
- IP address associated with each interface

#### Subnet

- subnet part: high order bits
- host part: low order bits

Device interfaces with same subnet part of IP address can physically reach each other without intervening router.

##### Recipe

- to determine the subnets, detach each interface form its host or router, creating isolated networks
- each isolated network is called a subnet

#### CIDR

Classless Inter Domain Routing

- subnet portion of address of arbitrary length
- address format: `a.b.c.d/x`
  - `x` is number of bits in subnet portion of address

#### DHCP

Dynamic Host Configuration Protocol

allow host to dynamically obtain its IP address from network server when it joins network

- can renew its lease on address in use
- allows reuse of address
  - only hold address while connection is on
- support for mobile users who want to join network

##### Overview

1. host broadcasts `DHCP discover` message (optional)
   - find where is DHCP server
2. DHCP server responds with `DHCP offer` message (optional)
   - offer an IP
3. host requests IP address with `DHCP request` message
   - host confirm
4. DHCP server sends address `DHCP ack` message
   - DHCP server confirm

DHCP can return more than IP address on subnet:

- address of first-hop router for client
- name and IP address of DNS server
- network mask
  - indicating network vs. host portion of address

##### example

1. laptop needs IP address: use DHCP
2. DHCP request encapsulated in **UDP**, encapsulated in IP, encapsulated in 802.1 Ethernet
3. Ethernet frame broadcast (destination MAC address: FF:FF:FF:FF:FF:FF (preserve for DHCP request)) on LAN, received at router running DHCP server
4. Ethernet demultiplexed to IP demultiplexed, UDP demultiplexed to DHCP
5. DHCP server formulates DHCP ACK containing:
   - client's IP address
   - IP address of first-hop router client
   - name & IP address of DNS server
6. encapsulation of DHCP server, frame forwarded to client, demultiplexing up to DHCP at client
7. client now knows:
   - its IP address
   - name and IP address of DNS server
   - IP address of its first-hop router

#### Hierarchical Addressing (Route Aggregation)

hierarchical addressing allows efficient advertisement of routing information

#### ICANN

Internet Corporation for Assigned Names and Numbers

- allocates address
- manages DNS
- assigns domain names, resolves disputes

### NAT

network address translation

- local network
  - with IP address `10.0.0.0/24`
  - datagrams with source or destination in this network have `10.0.0.0/24` address for source, destination
- Internet
  - all datagrams leaving local network have same single source NAT IP address (router Internet IP address), different source port numbers

local network uses just one IP address as far as outside world is concerned

- range of addresses not needed from ISP, just one IP address for all devices
- can change addresses of devices in local network without notifying outside world
- can change ISP without changing address of devices in local network
- devices inside local net are not explicitly addressable, visible by outside world

NAT router must:

- **outgoing datagrams**:
  replace every outgoing (source IP address, port number) datagram to (NAT IP address, new port number)
  remote servers will respond using (NAT IP address, new port number) as destination address
- remember every pair of translation pair
- **incoming datagrams**:
  replace destination field of every incoming datagram with corresponding pair stored in NAT table

16-bit port-number field

- 60000 simultaneous connections with a single LAN-side address

NAT is controversial

- router should only process up to layer 3
- address shortage should be solved by IPv6
- violates end-to-end argument
- NAT traversal: what if client wants to connect to server behind NAT?

### IPv6

- 32-bit address space soon to be completely allocated
- header format helps speed processing/forwarding
- header changes to facilitate QoS

#### IPv6 datagram format

changes from IPv4

- fixed-length 40 byte header
- no fragmentation allowed
- priority
  - identify priority among datagrams in flow
- flow label
  - identify datagrams in same "flow"
- next header
  - identify upper layer protocol for data
- checksum
  - removed entirely to reduce processing time at each hop
- options
  - allowed, but outside of header, indicated by "Next Header" field
- ICMPv6
  - addition messages types
  - multicast group management functions

#### Transition from IPv4 to IPv6

- not all routers can be upgrades
- tunneling
  - IPv6 datagram carried as payload in IPv4 datagram among IPv4 routers

## Generalized Forwarding and SDN

Each router contains a flow table that is computed and distributed by a logically centralized routing controller

### OpenFlow Data Plane Abstraction

- flow: defined by header files
- generalized forwarding: simple packet-handling rules
  - pattern: match values in packet header fields
  - action: for matched packet drop, forward, modify matched packet or send matched packet to controller
  - priority: disambiguate overlapping patterns
  - counters: number of bytes and number of packets

match+action: unified different kinds of devices

- Router
  - match: longest destination IP prefix
  - action: forward out a link
- switch
  - match: destination MAC address
  - action: forward or flood
- firewall
  - match: IP address and TCP/UDP port numbers
  - action: permit or deny
- NAT
  - match: IP address
  - action: rewrite address and port
