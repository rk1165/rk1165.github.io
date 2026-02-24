+++
title = 'Computer Networking Notes'
date = 2024-02-11

+++

### ARP

- ARP maps from IP to MAC address
- We need MAC address to send frames
- ARP table is local to machine. It is cached.

### TCP Tunneling

- Tunneling is the process of encapsulating content from a protocol A into another protocol B, usually because protocol A is blocked or unavailable.
- We take the TCP packet for data request and send it to the SSH of another server. We are smuggling data insided packet.
- Can this be achieved locally with two servers and a client.
- Application of TCP tunneling : Blocked Port
- Local port forwarding tunnel : create a local port
- SOCKS proxy (Dynamic port) tunneling

### Transfer file using netcat

- I run: `netcat -l -p 12345 > file.pdf`
- You run: `netcat $MY_IP_ADDRESS 12345 < file.pdf`

### ARP cache poisoning : How to steal packets on a wireless network!

- `sudo arpspoof -i wlan0 -t 192.168.0.13 192.168.0.1` `192.168.0.13` is my phone's IP address on the local network. `192.168.0.1` is the address of the router.
- What the above line of code basically does is "Hey phone! You want to send a packet to the router? Send that to me instead. Thanks!"
- This exploitation technique is called **ARP cache poisoning**.

### Implementing traceroute

- [Scapy](https://scapy.net/) is Python networking library which lets you construct packets really easily.
- IP packets have a `ttl` attribute. Every time a machine receives an IP packet, it decreases the `ttl` by 1 and passes it on.
- If a packet's `ttl` runs out before it replies, the last machine sends back an ICMP packet saying "sorry, failed!"
- There's also `tcptraceroute` which sends TCP packets.

### Spytools

- **netstat** tells what ports are open on computer : `netstat -tulpn`
- **dstat** tells how much data is actually being written to the physical hard drive right this second.
- **lsof** tells which files every process has open right now.
- [**ngrep**](https://dl.packetstormsecurity.net/papers/general/ngreptut.txt) and **tcpdump**
- **ftrace**

### Netflix's CDN

- CDN: Two concepts
  - Distributed caching of web assets
  - Intelligent rendezvous of client (e.g. web browsers) to appropriate cache.
- We created our own adaptive bitrate algorithms to adapt to changes in throughput
- Constantly polling the bit rate that they are able to maintain
- On the appliances : firmware contain OS, applications & scripts. The Firmware's OS: FreeBSD, Applications: nginx, BIRD BGP routing

### Notes

- `tracepath google.com` : to know the path mtu. maximum transfer unit
- `tc` : traffic control.
- Here's how we can use tc to make our personal internet slow

```bash
sudo tc qdisc add dev wlo1 root netem delay 500ms
# and turn it off with
sudo tc qdisc del dev wlo1 root netem
```

- `wlp3s0` is the network interface for wireless card.
- Every computer on the internet has a network card.
- When we make HTTP requests with ethernet/wifi, every packet gets sent to a MAC address.
- When you run `curl jvns.ca/cat.png`
  - curl calls the `getaddrinfo` function with `jvns.ca`
  - `getaddrinfo` finds the system DNS server.
  - `getaddrinfo` makes a DNS request to system DNS server and obtains the IP address.
- System's default DNS server is often configured in `/etc/resolv.conf`
- 4 common sockets type : TCP, UDP, raw, unix (to talk to programs on the same computer)
- How to know what order the packets should go in:
  - Every packet say what "range of bytes" it has. The position of the first byte is called the **sequence number**
- How to deal with lost packets:
  - When you get TCP data, you have to acknowledge it. If the server doesn't get an acknowledgement, it will _retry_ sending the data
- HTTP/2 is a binary protocol, you can make multiple requests at the same time, and you have to use TLS.
- TCP port 999 and UDP port 999 are different
- Subnet is the list of computers that you can talk to directly.
- When you send a packet to a computer in your subnet, you put the computer's MAC address on it. To get the right MAC, computer uses a protocol called ARP.
- `arp -na` to see the contents of the ARP table
- Routers use a protocol called **BGP** to decide what router the packet should go next.
- To describe groups of IP addresses use CIDR notation.
- In CIDR notation, a /n gives you 2<sup>32-n</sup> IP addresses.
- 10.9.0.0/24 is all the IP addresses which have the same first 24 bits as 10.9.0.0!
- To see the certificate for `jvns.ca` run `openssl s_client -connect jvns.ca:443 -servername jvns.ca`

```bash
nc example.com 80
HEAD / HTTP/1.1
```

- `HEAD` allows to get the headers of the file.
- `OPTIONS` is supposed to give the list of methods that are accepted on the current url
- `Content-Length` is a header that must be contained in every response and tells the browser the size of the body in the response.
- `Content-Type` is also a non-optional header and tells you what type the document has. This way the browser knows which parsing engine to spin up.
- `Last-Modified` is a header that contains the date when the document was last changed.
- most servers also send out an `ETag`. `ETag` stands for entity tag, and is a unique identifier that changes solely depending on the content of the file. Most servers actually use a hash function like SHA256 to calculate the ETag.
- `Cache-Control` is exactly what it sounds like. It allows the server to control how and for how long the client will cache the response it received.
- `If-Modified-Since` permits the server to skip sending the actual content of the document if it hasn’t been changed since the date provided in that header.
- REpresentational State Transfer
- `PUT` request updates an already present record in collection
- `POST` request creates new record and adds it to the collection
- IP allows us to talk to other machines on the internet while TCP allows us to have multiple streams of data between these two machines.
- TCP streams are distinguished by port numbers.
- **Head-of-line blocking (HOL blocking)** in computer networking is a performance-limiting phenomenon that occurs when a line of packets is held up by the first packet
- HOL blocking is, when one request is blocking other requests. A browser opens up upto six connections to a server.
- Request + Response : round-trip
- A proxy is the legitimate MITM and has many benefits such as:
  - saving bandwidth by adding additional compression
  - downsampling images.
  - aggressive caching.
- HTTPS = HTTP + TLS (SSL)
- A server identifies itself with a certificate that contains both the metadata about itself and fingerprint of its encryption key.
- HTTP/2 tackles HOL blocking.
- Header is compressed in HTTP/2
- multiplex: a system or signal involving simultaneous transmission of several messages along a single channel of communication
- combining multiple signals into a single signal.
- mixed-content problem: different assets such as images and iframes are sent using different protocol such as http rather than https.
- An origin comprises of scheme + host + port
- cross-origin fetch requests:
- Cross Origin Resource Sharing(CORS): CORS headers permit cross origin requests.
- If the Request header referer is on the Response header Access-Control-Allow-Origin it will be able to inspect the answer and use the data.
- Preflight requests:
- A CSRF token is an additional field appended to a form that has been put by the server and is stored in the server side as well.
- `netstat -an` : see the current state of sockets on your host
- `lsof -i -n` : gives the open Internet sockets when used with the -i option.
- 1MB = 1 _ 2^20 _ 8 bits
- 1Kb = 1000 bits
- In the terminal, you can use the **host** program to look up hostnames in DNS:
- IP addresses distinguish computers; port numbers distinguish _programs_ on those computers.

```bash
ncat 127.0.0.1 8000
GET / HTTP/1.1
Host: localhost
```

- Different status codes of HTTP:
  - **1xx - Informational.** The request is in progress or there's another step to take.
  - **2xx - Success!** The request succeeded. The server is sending the data the client asked for.
  - **3xx - Redirection.** The server is telling the client a different URI it should redirect to. The headers will usually contain a **Location** header with the updated URI. Different codes tell the client whether a redirect is permanent or temporary.
  - **4xx - Client error.** The server didn't understand the client's request, or can't or won't fill it. Different codes tell the client whether it was a bad URI, a permissions problem, or another sort of error.
  - **5xx - Server error.** Something went wrong on the server side.
- `ncat -l 9999` to listen on port 9999
- Web servers using `http.server` are made of two parts: the `HTTPServer` class, and a request handler class
- query parameters are written as `key=value` and separated by `&` signs
- `urllib.parse` module defines functions that fall into two broad categories: URL parsing and URL quoting.
- The URL parsing functions focus on splitting a URL string into its components, or on combining URL components into a URL string.
- `urllib.parse.urlparse(urlstring, scheme='', allow_fragments=True)` : Parse a URL into six components, returning a 6-tuple. This corresponds to the general structure of a URL: `scheme://netloc/path;parameters?query#fragment`
- HTTP URLs aren't allowed to contain spaces or certain other characters. So if you want to send these characters in an HTTP request, they have to be translated into a "URL-safe" or "URL-quoted" format.
- quoting means translating a string into a form that doesn't have any special characters in it, but in a way that can be reversed later
- Inside `do_POST`, our code can read the request body by calling the `self.rfile.read` method.
- `self.rfile.read` needs to be told how many bytes to read.
- A server usually needs to have a _stable (static) IP address_ so that clients can find it and connect to it.
- To set a cookie from a Python HTTP server, all you need to do is set the `Set-Cookie` header on an HTTP response
- Similarly, to read a cookie in an incoming request, you read the `Cookie` header
