# Beej's Guide to Network Programming

Using network socket

## What is a Socket?

- Everything in Unix is a file!
  - do any sort of I/O by **reading** or **writing** to a **file descriptor**
  - a **file descriptor** is a **integer** associated with an open file

## Two Types of Internet Sockets

### Stream Sockets

- `SOCK_STREAM`
- reliable two-way connected communication streams
  - error-free
  - send and receive in same order
- sample application
  - `telnet`
  - `HTTP` (Hypertext Transfer Protocol)
- use `TCP` protocol
  - `TCP/IP`, `IP` stands for "Internet Protocol"
  - `TCP` makes sure your data arrives sequentially and error-free.
  - `IP` deals with Internet routing.

### Datagram Sockets

- `SOCK_DGRAM`

- connectionless sockets
  - don't have to maintain an open connection
  - You just build a packet, slap an IP header on it with destination information, and send it out.
- If you send a datagram, it may arrive.
  - It may arrive **out of order**.
  - If it arrives, the data within the packet will be **error-free**.
- use `UDP` protocol
  - no connection needed
  - use when a `TCP` stack is unavailable
  - or when a few dropped packets here and there don’t mean the end of the Universe.
- sample application
  - `tftp`
    - trivial file transfer protocol
    - little brother to `ftp`
  - `dhcpcd`
    - a DHCP client
  - streaming audio/video
  - multiplayer games
- to ensure data send successfully, application may have their own protocol on top of `UDP`



