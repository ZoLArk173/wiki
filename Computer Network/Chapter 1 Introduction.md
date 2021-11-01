# Chapter 1 Introduction

## Protocol

- Define format, order of messages sent and received among network entities, and actions taken on message transmission, receipt.

## Internet connections

- ### DSL

  - Using existing **dedicated** telephone line to DSLAM
  - voice and data transmitted at different frequency over same line
  - < 2.5 Mbps up, < 24 Mbps down

- ### Cable Network

  - Using existing **shared** cable to ISP
  - TV signal and data transmitted at different frequency over same line
  - 2 Mbps up, 30 Mbps down
  - Since the cable is shared to multiple house, bandwidth must be sheared.

- ### Ethernet

  - Today, most systems connect to Internet via ethernet.

- ### Wireless LAN

  - within building
  - AP broadcast Wi-Fi signal.

- ### Cellular Network

  - Provided by cellular operator.
  - 3G, 4G, LTE, 5G

## Packet

- Message breaks int smaller chunks, known as **packets**.

- Store and forward
  - Entire packet must arrive at router before it can be transmitted on next link.
  - If arrival rate **exceeds** transmission rate, packets will **queue**.
  - Packets can be **dropped** if memory buffer fills up.

## Key network-core functions

- ### Routing

  - determines source-destination route taken by packets.

- ### Forwarding

  - move packets from input to appropriate router output.

- ### Circuit switching

  - Instead of routing packets, circuit switching provide dedicate connection between switches.
  - If there are more users, circuit switching is not efficient because it have to provide every users their own connection path.

## Loss and delay

- delay = nodal processing + queueing delay + transmission delay + propagation delay
  - nodal processing
    - check bit errors
    - determine output link
  - queueing delay
    - waiting for transmission
  - transmission delay
    - **time that put packet into link**
    - packet length / link bandwidth
  - propagation delay
    - **time that transmit from end to end**
    - link length / propagation speed

- Packet arriving to full queue dropped.
- Lost packet may be retransmitted by previous node, by source, or not at all.

## Internet protocol stack

### Why layering?

- Easy to maintenance and update

``` sequence
title: Inter Prorocol Stack 
participant application as a
participant transport as t
participant network as n
participant link as l
participant physical as p
note over a: HTTP, FTP
a->t:
note over t: TCP, UDP
t->n:
note over n: IP, routing protocols
n->l:
note over l: Ethernet, WiFi
l->p:
note over p: bits
```

- application
  - supporting network applications
- transport
  - process data transfer
- network
  - routing datagrams from source to destination
- link
  - data transfer between neighboring network elements
- physical

### ISO reference model

Add two layers between `application` and `transport`.

- presentation
  - allow applications to interpret meaning of data. (encryption)
- session
  - synchronization, checkpointing, recovery of data exchange.

## Network Security

Internet not originally designed with security in mind.
Security considered in all layers.

- ### Denial of Service (DoS)
  - Overwhelming resource server with traffic to make it unavailable
  
- ### Packet sniffing

  - Broadcast media
  - Reads and records all packets passing by
  - Wireshark

- ### IP spoofing

  - send packet with false source address
