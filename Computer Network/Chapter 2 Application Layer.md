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
- In network stack, `transport` layer and below are controlled by OS, and socket lies between `application` and `transport`.

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

  - ```sequence
    title: Non-presistent HTTP
    participant client as c
    participant server as s
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

- can be use for: authorization, shopping carts, recommendations

- ```sequence
  title: Cookies
  participant client as c
  participant server(amazon) as s
  note left of c: ebay 8734
  c->s: usual HTTP request
  note right of s: creates ID 1678\nSave to database
  s->c: usual HTTP respond\ncookie: 1678
  note left of c: ebay 8734\namazon 1678
  c->s: usual HTTP request\ncookie: 1678
  s->c: usual HTTP respond
  ```

## Web Cache (proxy server)

satisfy client request without involving origin server.

- reduce response time for client request
- reduce traffic
- enables poor server to effectively deliver contents

- conditional GET
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

