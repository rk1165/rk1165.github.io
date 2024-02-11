+++
title = 'High Performance Browser Networking'
date = 2024-02-11

+++

## Primer on Latency and Bandwidth

- _Latency_ is the time it takes for a message, or a packet, to travel from its point of origin to the point of destination.
- _Propagation delay_ time required for a message to travel from the sender to receiver, which is a function of distance over speed with which the signal propagates.
- _Transmission delay_ time required to push all the packet’s bits into the link, which is a function of the packet’s length and data rate of the link.
- _Processing delay_ time required to process the packet header, check for bit-level errors, and determine the packet’s destination.
- _Queuing delay_ time the packet is waiting in the queue until it can be processed.
- The total latency between the client and the server is the sum of all the delays just listed.
- Network data rates are typically measured in bits per second (bps), whereas data rates for non-network equipment are typically shown in bytes per second (Bps).
- [Controlling Queue Delay](https://hpbn.co/aqmacm)
- great-circle path (the shortest distance between two points on the globe)
- _Bandwidth_ Maximum throughput of a logical or physical communication path.
- we need to reduce round trips, move the data closer to the client, and build applications that can hide the latency through caching, pre-fetching, and a variety of similar techniques

## Building Blocks of TCP

- All TCP connections begin with a three-way handshake.
- _SYN_ Client picks a random sequence number x and sends a SYN packet, which may also include additional TCP flags and options
- _SYN ACK_ Server increments x by one, picks own random sequence number y, appends its own set of flags and options, and dispatches the response.
- _ACK_ Client increments both x and y by one and completes the handshake by dispatching the last ACK packet in the handshake.
- Before the client or the server can exchange any application data, they must agree on starting packet sequence numbers, as well as a number of other connection specific variables, from both sides. The sequence numbers are picked randomly from both sides for security reasons.
- each new connection will have a full roundtrip of latency before any application data can be transferred.
- The delay imposed by the three-way handshake makes new TCP connections expensive to create, and is one of the big reasons why connection reuse is a critical optimization for any application running over TCP
- TCP Fast Open (TFO) is a mechanism that aims to eliminate the latency penalty imposed on new TCP connections by allowing data transfer within the SYN packet.
- Multiple mechanisms were implemented in TCP to govern the rate with which the data can be sent in both directions:
  - flow control
  - congestion control
  - congestion avoidance.

### Flow control

- Flow control is a mechanism to prevent the sender from overwhelming the receiver with data it may not be able to process.
- To address this, each side of the TCP connection advertises its own receive window (rwnd), which communicates the size of the available buffer space to hold the incoming data.
- each ACK packet carries the latest rwnd value for each side, allowing both sides to dynamically adjusts the data flow rate to the capacity and processing speed of the sender and receiver.
- The original TCP specification allocated 16 bits for advertising the rwnd size which places a hard upper bound on the maximum value that can be advertised.
- To address this, RFC 1323 was drafted to provide a "TCP window scaling" option, which allows us to raise the maximum window size from 65535 bytes to 1 GByte.
- The window scaling setting can be checked and enabled via the following commands:
  - `sysctl net.ipv4.tcp_window_scaling`
  - `sysctl -w net.ipv4.tcp_window_scaling=1`
