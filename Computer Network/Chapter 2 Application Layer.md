# Chapter 2 Application Layer

## Architectures

- ### client to server

  - always-on host
  - data center
  - server has permanent IP address
  - clients don't communicate directly with each other

- ### Peer to Peer (P2P)

  - not always-on host
  - arbitrary end systems directly communicate
  - peers request services from other peers
  - decentralization

## Sockets

- Process sends and receives data from its socket.
- sockets just like doors
- In [[Chapter 1 Introduction#Internet protocol stack|network stack]], `transport` layer and below are controlled by OS, and socket lies between `application` and `transport`.

## Address

to receive messages, process must have **identifier**.

identifier includes **IP** and **port number**.

## What transport services does an app need?

- data integrity
- timing
- throughput
- security

## Internet transport protocols services

- ## TCP

  - reliable
  - flow control
    - sender won't overwhelm receiver
  - congestion control
    - throttle sender when network overloaded
  - connection oriented
    - setup required between server and client
  - Good for file transfer, which need to ensnare file correctness.

- ### UDP

  - not reliable
  - Good for video conference calls, since small part of data lost doesn't effect the service.

- ### SSL

  - Provides encrypted TCP connection, data integrity, end-point authentication.
  - An application layer.

## HTTP

- use TCP, default port is 80.
- Stateless
  - server maintains no information about past client requests

### connections

- #### none-persistent HTTP

  - send one object per one TCP connection

  - downloading multiple objects require multiple TCP connections

  - ```mermaid
    sequenceDiagram
    participant c as client
    participant s as server
    c->s: initiates TCP connection
    s->c: accepts connection, notifying client
    c->s: sends HTTP request message into TCP connection socket
    s->c: forms response and sends message into its socket
    note left of s: TCP conection closed
    note right of c: receives messages
    note over s, c: repeat for each object
    ```

  - OS overhead for each TCP connection

  - browsers often parallel TCP connections to fetch objects

- #### persistent HTTP

  - multiple objects can transfer between client and server with one TCP connection.
  - server leaves connection open after sending response.
  - subsequent HTTP messages send over same connections.

### HTTP message

- #### request

  - Human readable format
  - Method
    - GET
    - POST
    - HEAD
    - PUT (HTTP 1.1)
    - DELETE (HTTP 1.1)

- #### response

  - status code

## Cookies

- keep and manage on user's browser

- can be use for: authorization, shopping carts, recommendations, etc.

- ```mermaid
  sequenceDiagram
  participant c as client
  participant s as server(amazon)
  note left of c: ebay 8734
  c->>s: usual HTTP request
  note right of s: creates ID 1678<br/>Save to database
  s->>c: usual HTTP respond<br/>cookie: 1678
  note left of c: ebay 8734<br/>amazon 1678
  c->>s: usual HTTP request<br/>cookie: 1678
  s->>c: usual HTTP respond
  ```

## Web Cache (proxy server)

satisfy client request without involving origin server.

- reduce response time for client request
- reduce traffic
- enables poor server to effectively deliver contents
- conditional `GET`
  - don't send objects if cache has up to date version

## Email

### User agent

- mail reader
- composing, editing, reading email messages

### Mail server

- **mailbox** contents incoming messages for user
- **message queue** for outgoing mail messages
- **SMTP protocol** between mail server to send mail messages

### SMTP

- uses TCP port 25
- phases of transfer
  - handshaking
  - messages
  - closure
- command/response act like HTTP
- messages must be in 7-bit ASCII
  - other language must encode before sending

### Mail access protocols

Different from SMTP, mail access protocols are for client fetching mails from server.

- POP
  - only authorization and download
- IMAP
  - more features including modify messages on server.
- HTTP
  - Gmail

## DNS

Map between IP address and domain name.

- distributed database
  - if client wants IP for `www.amazon.com`
    - client queries `root DNS server` to find `com DNS server`
    - client queries `com DNS server` to get `amazon.com DNS server`
    - client queries `amazon.com DNS server` to get IP address of `www.amazon.com`
- application-layer protocal
  - hosts, name servers communicate to **resolve** names.
- host aliasing
- load distribution
  - many IP addresses correspond to one domain name

### Root Name Server

- contract by local name server if it cannot resolve the name
  - gets mapping
  - returns a list of TLD server to local name server

### Top-Level Domain Servers (TLD)

- responsible for com, org, net, edu...
- top-level country domains: uk, ca, jp..

### Local DNS Name Server

- does not strictly belong to hierarchy
- each ISP, university, company has one
- when makes DNS query, query is usually sent to its local DNS server
  - local cache
  - acts as proxy, forwards query to other DNS server if it is not cached

### Caching

- cashing entries timeout after some time
- TLD servers typically cached in local name servers
- may be out of date

### Records

- DNS records save in resource records format (RR)
- RR format: (`name`, `value`, `type`, `ttl`)
  - type = A
    - `name` is hostname
    - `value` is IP address
  - type = NS
    - DNS server
    - `name` is domain
    - `value` is hostname
    - since it save hostname, you might need to query twice
      - first query get hostname, second query get IP address
  - type = CNAME
    - `name` is alias for some real name
    - `value` is real name
  - type = MX
    - `value` is name of mail sever associated with name

### Protocol

query and reply message with same format

- message header
  - identification
    - 16 bit for identify query, reply to query use same id
  - flags
    - query or reply
    - recursion desired
    - recursion available
    - reply is authoritative
- questions
- answers
  - RR in response to query
- authority
- additional info

### Attacking DNS

- DDoS
- redriect attacks
  - man-in-middle intercept queries
  - send fake relies to DNS server, which caches
- exploit DNS for DDoS
  - send queries with spoofed source address

