# Transport Layer: TCP and UDP

## Overview of TCP/IP Protocols

```mermaid
graph TD
subgraph first
    id1.1(tcpdump)
    id1.2(mrouted)
    id1.3(ping)
    id1.4(traceroute)
    id1.5(applications)
end
subgraph second
    id2.1(IGMP)
    id2.2(ICMP)
    id2.3(TCP)
    id2.4(UDP)
    id2.5(ICMPv6)
end
subgraph third
    id3.1(ARP/RARP)
    id3.2(IPv4)
    id3.3(IPv6)
end
subgraph forth
    id4.1(BPG/DLPI)
    id4.2(datalink)
end

id1.1-->id4.1
id1.2-->id2.1
id1.3-->id2.2
id1.3-->id2.5
id1.4-->id2.2
id1.4-->id2.4
id1.4-->id2.5
id1.4-->id3.3
id1.5-->id2.3
id1.5-->id2.4
id2.1-->id3.2
id2.2-->id3.2
id2.3-->id3.2
id2.3-->id3.3
id2.4-->id3.2
id2.4-->id3.3
id2.5-->id3.3
id3.1-->id4.2
id3.2-->id4.2
id3.3-->id4.2
id4.1-->id4.2
```

## Comparing TCP and UDP

|                                   | TCP                          | UDP                     |
| ---------------------------------:|:----------------------------:|:-----------------------:|
| binding Between client and server | Yes<br />connection-oriented | No<br />connection-less |
| data                              | byte-stream                  | record                  |
| reliability                       | Yes<br />ack, time-out, retx | No                      |
| sequencing                        | Yes                          | No                      |
| flow control                      | Yes<br />window-based        | No                      |
| full-duplex                       | Yes                          | Yes                     |

## TCP Connection

### Establishment

#### Three-way handshake

```mermaid
sequenceDiagram
participant c as client
participant s as server
note right of s: socket, blind, listen<br>LISTEN (passive open)<br>accept (blocks)
note left of c: socket<br>connect (blocks)<br>(active open) SYN_SENT
c->>s: SYN j
note right of s: SYN_RCVD
s->>c: SYN k, ack j+1
note left of c: ESTABLISHED<br>connect returns
c->>s: ack k+1
note right of s: ESTABLISHED<br>accept returns<br>read (blocks)
```

### Data Transfer

```mermaid
sequenceDiagram
participant c as client
participant s as server
note left of c: write<br>read (blocks)
c->>s: request
note right of s: read returns<br>(server processes request)
note right of s: write<br>read (blocks)
s->>c: reply<br>ack of request
note left of c: read returns
c->>s: ack of reply
```

### Termination

#### Four-way handshake

```mermaid
sequenceDiagram
participant c as client / server
participant s as server / client
note left of c: close<br>(active close) FIN_WAIT_1
c->>s: FIN m
note right of s: COLSE_WAIT (passive close)<br>read returns 0
s->>c: ack m+1
note left of c: FIN_WAIT_2
note left of c: TIME_WAIT
activate c
note left of c: 1~4 mins
note right of s: close<br>LAST_ACK 
s->>c: FIN n
c->>s: ack n+1
note right of s: CLOSED
deactivate c
note left of c: CLOSED
```

`TIME_WAIT` to allow old duplicate segment to expire for reliable termination. (the end performing active close might have to retx the final ACK)

### TCP State Transition Diagram

## Allocation of Port Numbers

- IANA (Internet Assigned Numbers Authority)
  - 1 ~ 1023
    - IANA well-known ports
  - 1024 ~ 49151
    - IANA registered ports
  - 49152 ~ 65535
    - IANA dynamic or private ports
- BSD
  - 1 ~ 1023
    - BSD reserved ports
  - 1024 ~ 50000
    - BSD **ephemeral** (short period of time) ports 
  - 50001 ~ 65535
    - BSD nonprivileged servers
- Solaris
  - 513 ~ 1023
    - Solaris reserved ports
  - 32768 ~ 65535
    - Solaris ephemeral ports
