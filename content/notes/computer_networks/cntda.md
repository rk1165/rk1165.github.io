+++
title = 'Computer Networks - A Top Down Approach'
date = 2024-02-11

+++

## Principles of Network Applications

- Some of the better-known applications with a client-server architecture include the Web, FTP, Telnet, and e-mail.
- In a P2P architecture, the application exploits direct communication between pairs of intermittently connected hosts, called _peers_.
- P2P applications include file sharing (BitTorrent), peer-assisted download acceleration (e.g. Xunlei), and Internet telephony and video conference (Skype)
- Some apps have hybrid architectures. For many instant messaging apps, servers are used to track the IP addresses of users, but user-to-user messages are sent directly between user hosts.
- P2P apps face challenges of security, performance, and reliability due to their highly decentralized structure.
- Processes on two different end systems communicate with each other by exchanging **messages** across the computer network.
- A process in a P2P file-sharing system can both upload and download files.
- In the context of a communication session between a pair of processes, the process that initiates the communication is labeled as the client. The process that waits to be contacted to begin the session is the server.
- A process sends messages into, and receives messages from, the network through a software interface called a **socket**
- To identify the receiving process, two pieces of information need to be specified:
  1. the address of the host : In the Internet **IP address**
  2. an identifier that specifies the receiving process in the destination host **port number**
- Services that a transport-layer protocol can offer to applications invoking it can be broadly classifed along four dimensions
  - reliable data transfer
  - throughput
  - timing
  - security
- If a protocol provides a guaranteed data delivery service, it is said to provide **reliable data transfer**
- Applications that have throughput requirements are said to be **bandwidth-sensitive applications**
- An example timing guarantee might be that every message that the sender pumps into the socket arrives at the receiver's socket no more than 100 msec later.
- The Internet makes two transport protocols available to apps, UDP and TCP.

### TCP

- The TCP service model includes a connection-oriented service and a reliable data transfer service.
- After the handshaking phase, a TCP connection is said to exist b/w the sockets of the two processes.
- The connection is a full-duplex connection in that the two processes can send messages to each other over the connection at the same time.
- TCP also includes a congestion-control mechanism, a service for the general welfare of the Internet.
- The Internet community has developed an enhancement for TCP, called **Secure Sockets Layer (SSL)**.
- TCP-enhanced-with-SSL not only does everything that traditional TCP does but also provides critical process-to-process security services, including encryption, data integrity, and end-point authentication.
- SSL has its own socket API.

### UDP

- UDP is a no-frills, lightweight transport protocol, providing minimal services. UDP is connectionless, so there is no handshaking before the two processes start to communicate
- UDP provides an unreliable data transfer service.
- throughput or timing guarantees — services not provided by today’s Internet transport protocols

### Application-Layer Protocols

- In particular an application-layer protocol defines:
  - The types of messages exchanged, for example, request messages and response messages.
  - The syntax of the various message types, such as the fields in the message and how the fields are delineated.
  - The semantics of the fields, that is, the meaning of the information in the fields.
  - Rules for determining when and how a process sends messages and responds to messages.

## The Web and HTTP

- HTTP is defined in RFC 1945 and RFC 2616.
- Because an HTTP server maintains no information about the clients, HTTP is said to be a **stateless protocol**.
- Should each request/response pair be sent over a _separate_ TCP connection, or should all of the requests and their corresponding responses be sent over the _same_ TCP connection?
- In the former approach, the application is said to use **non-persistent connections**; and in the later approach, **persistent connections**.
- HTTP uses persistent connections in its default mode with pipelining.
- Use of non-persistent connections, where each TCP connection is closed after the server sends the object—the connection does not persist for other objects
- **Round-trip time (RTT)**, which is the time it takes for a small packet to travel from client to server and then back to the client.
- Most recently, HTTP/2 RFC 7540 builds on HTTP 1.1 by allowing multiple requests and replies to be interleaved in the _same_ connection, and a mechanism for prioritizing HTTP message requests and replies within this connection.
- When a server receives a request with the HEAD method, it responds with an HTTP message but it leaves out the requested object.
- The PUT method is often used in conjunction with Web publishing tools. It allows a user to upload an object to a specfic path (directory) on a specfic Web server.
- The DELETE method allows a user, or an application, to delete an object on a Web server.
- The _Last-Modified:_ header is critical for object caching, both in the local client and the network cache servers (also known as proxy servers).
- **Cookies**, defined in RFC 6265, allow sites to keep track of users.
- Cookie technology has four components:
  1. a cookie header line in the HTTP response message
  2. a cookie header line in the HTTP request message
  3. a cookie file kept on the user's end system and manged by the user's browser
  4. a back-end database at the Web site.
- When the request comes into the Amazon Web server, the server creates a unique identification number and creates an entry in its backend database that is indexed by the identification number. The Amazon Web server then responds to Susan’s browser, including in the HTTP response a `Set-cookie:` header, which contains the identification number
- When Susan’s browser receives the HTTP response message, it sees the `Set-cookie:` header. The browser then appends a line to the special cookie file that it manages. This line includes the hostname of the server and the identification number in the `Set-cookie:` header.
- A **Web cache** - also called a **proxy server** - is a network entity that satisfies HTTP requests on the behalf of an origin Web server.
- The Web cache has its own disk storage and keeps copies of recently requested objects in this storage.
- A user's browser can be configured so that all of the user's HTTP requests are first directed to the Web cache.
- A cache is both a server and a client at the same time.
- Web caches can substantially reduce Web traffic in the Internet as a whole.
- Hit rates - the fraction of requests thats are satisfied by a cache - typically range from 0.2 to 0.7 in practice.
- A CDN company installs many geographically distributed caches throughout the Internet, thereby localizing much of the traffic.
- HTTP has a mechanism that allows a cache to verify that its objects are up to date. This mechanism is called **conditional GET**
- An HTTP request message is a so-called conditional GET message if
  1.  the request message uses the GET method
  2.  the request message includes an `If-Modified-Since:` header line

### Electronic Mail in the Internet

- The Internet mail system has three major components: **user agents**, **mail servers**, and the **Simple Mail Transfer Protocol (SMTP)**
- Each recipient has a **mailbox** located in one of the mail servers.
- SMTP transfers messages from senders' mail servers to the recipients' mail servers.
- SMTP restricts the body (not just the headers) of all mail messages to simple 7-bit ASCII.
- SMTP uses persistent connections. It is a **push** protocol
- There are currently a number of popular mail access protocols, including **Post Office Protocol - Version 3 (POP3)**, **Internet Mail Access Protocol (IMAP)**, and HTTP
- POP3 is defined in RFC 1939.
- POP3 begins when the client opens a TCP connection to the mail server (the server) on port 110.
- POP3 progresses through three phases: authorization, transaction, and update.
- IMAP is defined in RFC 3501.
- Unlike POP3, an IMAP server maintains user state information across IMAP sessions.

### DNS - The Internet's Directory Service

- The DNS is
  1. a distributed database implemented in a hierarchy of DNS servers.
  2. an application-layer protocol that allows hosts to query the distributed database.
- The DNS servers are often UNIX machines running the Berkeley Internet Name Domain (BIND) software.
- The DNS protocol runs over UDP and uses port 53.
- DNS provides a few other important services in addition to translating hostnames to IP addresses:
- **Host aliasing** `relay1.west-coast.enterprise.com` could have two aliases such as `enterprise.com` and `www.enterprise.com`. In this case `relay1.west-coast.enterprise.com` is said to be a **canonical hostname**. DNS can be invoked by an application to obtain the canonical hostname for a supplied alias hostname as well as the IP address of the host.
- **Mail server aliasing**
- **Load distribution** DNS is also used to perform load distribution among replicated servers, such as replicated Web servers. It stores the list of all IP addresses associated with replicated servers and when requested provides the one at the top and rotates the list.
- To a first approximation, there are three classes of DNS servers - root DNS servers, top-level domain (TLD) DNS servers, and authoritative DNS servers - organized in a hierarchy.
- Root DNS servers : over 400 scattered around the world. Managed by 13 different organizations. They provide the IP addresses of the TLD servers
- TLD servers provide IP addresses for authoritative DNS servers.
- Authoritative DNS servers : Every organization with publicly accessible hosts (such as Web servers and mail servers) on the Internet must provide publicly accessible DNS records that map the names of those hosts to IP addresses. An organization’s authoritative DNS server houses these DNS records
- another important DNS server **local DNS server** does not strictly belong to the hierarchy but central to the DNS architecture. Each ISP — residential or institutional has a local DNS server (also called a default name server). When a host connects to an ISP, the ISP provides the host with the IP addresses of one or more of its local DNS servers (typically through DHCP)
- **DNS caching** The idea behind DNS caching is very simple. In a query chain, when a DNS server receives a DNS reply (containing, for example, a mapping from a hostname to an IP address), it can cache the mapping in its local memory
- The DNS servers that together implement the DNS distributed database store **resource records (RRs)**, including RRs that provide hostname-to-IP address mappings.
- DNS RFC 1034 and RFC 1035
- A resource record is a four-tuple that contains the following fields: `(Name, Value, Type, TTL)`
- If `Type=A` then `Name` is a hostname and `Value` is the IP address for the hostname.
- If `Type=NS` then `Name` is a domain (such as foo.com) and `Value` is the hostname of an authoritative DNS server that knows how to obtain the IP addresses for hosts in the domain.
- If `Type=CNAME`, then `Value` is a canonical hostname for the alias hostname `Name`
- If `Type=MX`, then `Value` is the canonical name of a mail server that has an alias hostname `Name`
- To send a DNS query message directly from the host - can be done with **nslookup**
- A **registrar** is a commercial entity that verifies the uniqueness of the domain name, enters the domain name into the DNS database, and collects a small fee for its services.
- A more effective DDoS attack against DNS would be to send a deluge of DNS queries to top-level-domain servers.
- DNS has four components
  - DNS Recursors : The first one the client contacts for DNS query. Ex. 8.8.8.8, 1.1.1.1
  - Root DNS servers : 13 Root DNS servers. (13 diff. IPs). Every DNS resolver on the planet has these 13 IPs programmed into them
  - TLD Nameservers : These contain information about domains under that TLD.
  - Authoritative nameservers : These are the actual DNS servers with DNS records. e.g godaddy's servers, cloudflare DNS, your own DNS server in the internet

## Peer-to-Peer File Distribution

- In BT lingo, the collection of all peers participating in the distribution of a particular file is called a _torrent_
- Each torrent has an infrastructure node called a _tracker_.
- When a peer joins a torrent, it registers itself with the tracker and periodically informs the tracker that it is still in the torrent. In this manner, the tracker keeps track of the peers that are participating in the torrent.
- A Distriubted Hash Table **DHT** is a simple database, with the database records being distributed over the peers in P2P system.
- DHT - Distribute (key, value) pairs over millions of peers. Pairs are evenly distriubted over peers.
- (Peer to peer streaming)

## Video Streaming and Content Distribution Network

- The most important performance measure for streaming video is average end-to-end throughput.
- Dynamic Adaptive Streaming over HTTP (DASH). In DASH, the video is encoded into several different versions, with each version having a different bit rate and, correspondingly, a different quality level.
- (P2P CDN)
- CDNs typically adopt one of two different server placement philosophies
- **Enter Deep** Pioneered by Akamai, its aim is to _enter deep_ into the access networks of the ISPs, by deploying server clusters in access ISPs all over the world.
- **Bring Home** In this the aim is to _bring the ISPs home_ by building large clusters at a smaller number of sites.
- When a browser in user's host is instructed to retrieve a specific video, the CDN must intercept the request so that it can:
  1. determine a suitable CDN server cluster for that client at that time
  2. redirect the cleints' request to a server in that cluster.
- At the core of any CDN deployment is a **cluster selection strategy**, that is, a mechanism for dynamically directing clients to a server cluster or a data center within the CDN.

## Socket Programming : Creating Network Applications

- sending process attaches to the packet a destination address, which consists of destination host's IP address and the destination socket's port number.
- The sender's source address consisting of the IP address of the source host and the port number of the source socket are also attached to the packet (done by underlying OS)
- The `socket` module forms the basis of all network communications in Python.
- `clientSocket = socket(AF_INET, SOCK_DGRAM)` - creates the client's socket.
- The first parameter indicates the address family; in particular, `AF_INET` indicates that the underlying network is using IPv4.
- The second parameter indicates that the socket is of type `SOCK_DGRAM`, which means it is a UDP socket.
- `serverSocket.bind(('', serverPort))` - binds the port number to the server's socket.
- TCP connection has welcoming socket with which the client does a three-way handshake.
- When the TCP server "hears" the knocking, it creates a new door - more precisely, a _new_ socket that is dedicated to that particular client.
- `clientSocket = socket(AF_INET, SOCK_STREAM)` the second parameter `SOCK_STREAM` indicates a TCP socket.
- `clientSocket.connect((serverName, serverPort))` This line initiates the TCP connection between the client and server.
- `serverSocket.listen(1)` this line has the server listen for TCP connection requests from the client. The parameter specifies the maximum number of queued connections.
- `connectionSocket, addr = serverSocket.accept()` when a client knocks on this door, the program invokes the `accept()` method for serverSocket, which creates a new socket in the server, called `connectionSocket`, dedicated to this particular client.
