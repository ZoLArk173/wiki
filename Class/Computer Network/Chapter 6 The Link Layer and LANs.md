# Chapter 6 The Link Layer and LANs

## Introduction

### Terminology

- nodes: hosts and routers
- links: communication channels that connect adjacent nodes along communication path
  - wired links
  - wireless links
  - LANs
- layer-2 packet: frame, encapsulates datagram

### Context

data-link layer has responsibility of transferring datagram from one node to physically adjacent node over a link

- datagram transferred by different link protocols over different links
- each link protocol provides different services

### Services

- framing, link access:
  - encapsulate datagram into frame, adding header, trailer
  - channel access if shared medium
  - "MAC" addresses used in frame headers to identify source, destination
    - different from IP address
- reliable delivery between adjacent nodes
  - seldom used on low bit-error link
  - wireless links: high error rates
  - different from transport layer error detection, link error detection verifies the **integrity** of frames when they transmitted over the media. Transport layer error detection operates when receiver receive packets, since sender divides data into packets, there may be packet loss, and transport layer can ask for retransmission.

- flow control
  - pacing between adjacent sending and receiving nodes

- error detection
  - errors causes by digital attenuation, noise
  - receiver detects presence of errors
    - signals sender for retransmission or drops frame
- error correction
  - receiver identifies and corrects bit error without resorting to retransmission
- half-duplex and full-duplex
  - with half duplex, nodes at both ends of link can transmit, but not at same time

### Implemented

- link layer implemented in adaptor or on a chip
  - Ethernet card, 802.11 card
  - implements link, physical layer
- attaches into host's system buses
- combination of hardware, software, firmware

### Adaptors Communicating

sending side:

- encapsulates datagram in frame
- adds error checking bits, rdt, flow control, etc.

receiving side:

- looks for errors, rdt, flow control, etc.
- extracts datagram, passes to upper layer at receiving side

## Error Detection, Correction

### Error Detection

error detection not 100% reliable!

- protocol may miss some errors, but rarely
- larger EDC field yields better detection and correction

#### Parity Checking

- single bit parity
  - detect single bit errors
- two-dimensional bit parity
  - detect and correct single bit errors

#### Internet Checksum (transport layer)

detect errors in transmitted packet

sender: 

- treat segment contents as sequence of 16-bit integers
- checksum: addition (1's complement) of segment contents
- sender puts checksum value into UDP checksum field

receiver:

- compute checksum of received segment
- check if computed checksum equals checksum field value

#### Cyclic Redundancy Check

- more powerful error-detection coding

- view data bits, `D`, as a binary number

- choose `r + 1` bit pattern (generator), `G`

- choose `r` CRC bits, `R`, such that

  - `<D, R>` exactly divisible by `G`
  - receiver knows G, divides `<D, R>` by `G`
    - if non-zero reminder, error detected!
  - can detect all burst errors less than `r + 1` bits

- widely used in practice (Ethernet, 802.11 WiFi, ATM)

- mathematical formula:
  $$
  D*2^r\ XOR\ R
  $$

example

# QAQ

## Multiple Access Protocols

two types of links:

- point-to-point
  - PPP for dial-up access
  - point-to-point link between Ethernet switch, host
- broadcast (share wire or medium)
  - old-fashioned Ethernet
  - upstream HFC
  - 802.11 wireless LAN
  - single sheared broadcast channel
  - two or more simultaneous transmissions by nodes interface
    - **collision** if node receives two or more signals at the same time

multiple access protocol

- distributed algorithm that determines how nodes share channel (determine when node can transmit)
- communication about channel sharing must be use channel itself
  - no out-of-band channel for coordination

### Ideal Multiple Access Protocol

broadcast channel for rate R bps

- When one node wants to transmit, it can send at rate `R`.
- When `M` nodes want to transmit, each can send at average rate `R/M`
- fully decentralized
  - no special node to coordinate transmissions
- simple

### MAC Protocols: Taxonomy

three broad class:

- channel partitioning
  - divide channel into smaller pieces (time slots, frequency, code)
  - allocate piece to node for exclusive use
- random access
  - channel not divided, allow collisions
  - recover from collisions
- taking turns
  - nodes take turns, but nodes with more to send can take longer turns

#### Channel Partition MAC Protocols

##### TDMA

time division multiple access

- access to channel in rounds
- each section gets fixed length slot (packet transmission time) in each round
- unused slot go idle

##### FDMA

frequency division multiple access

- channel spectrum divided into frequency bands
- each station assigned fixed frequency band
- unused transmission time in frequency bands go idle

#### Random Access Protocols

- when node has packet to send

  - transmit at full channel data rate R

  - no a priori coordination among node

- two or more transmitting nodes -> collision

- random access MAC protocol specifies:

  - how to detect collisions
  - how to recover from collisions (via delayed retransmissions)

- examples of random access MAC protocols:

  - slotted ALOHA
  - ALOHA
  - CSMA, CSMA/CD, CSMA/CA

##### Slotted ALOHA

assumptions:

- all frames are same size
- time divided into equal size slots (time to transmit 1 frame)
- nodes start to transmit only slot beginning
- if 2 or more node transmit in slot, all nodes detect collision

operation:

- when node obtains fresh frame, transmits in next slot
  - if no collision, node can send new frame in next slot
  - if collision, node retransmits frame in each subsequent slot with probability `p` until success 

Pros:

- single active node can continuously transmit at full rate of channel
- highly decentralized, only slots in nodes need to be in sync
- simple

Cons:

- collisions, wasting slots
- idle slots
- nodes may be able to detect collision in less than time to transmit packet
- clock synchronization

efficiency:

channel used for useful transmissions 37% of time at best

##### Pure (unslotted) ALOHA

- simpler, no synchronization
- when frame first arrives
  - transmit immediately
- collision probiability increases

efficiency:

18%, worse than slotted ALOHA