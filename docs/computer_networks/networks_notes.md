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

### Networking

- tracepath google.com : to know the path mtu. maximum transfer unit
- tc : traffic control.
- Here's how we can use tc to make our personal internet slow

```bash
sudo tc qdisc add dev wlo1 root netem delay 500ms
# and turn it off with
sudo tc qdisc del dev wlo1 root netem
```

- wlp3s0 is the network interface for wireless card.
- Every computer on the internet has a network card.
- When we make HTTP requests with ethernet/wifi, every packet gets sent to a MAC address.
- When you run `curl jvns.ca/cat.png`
  - curl calls the `getaddrinfo` function with `jvns.ca`
  - `getaddrinfo` finds the system DNS server.
  - `getaddrinfo` makes a DNS request to system DNS server and obtains the IP address.
- System's default DNS server is often configured in `/etc/resolv.conf`
- 2 kinds of DNS servers:
  - Recursive : can get any IP address by asking the right authoritative server
  - Authoritative
- 4 common sockets type : TCP, UDP, raw, unix (to talk to programs on the same computer)
- How to know what order the packets should go in:
  - Every packet say what "range of bytes" it has. The position of the first byte is called the **sequence number**
- How to deal with lost packets:
  - When you get TCP data, you have to acknowledge it. If the server doesn't get an acknowledgement, it will _retry_ sending the data
- HTTP/2 is a binary protocol, you can make multiple requests at the same time, and you have to use TLS.
- TCP port 999 and UDP port 999 are different
- Subnet is the list of computers that you can talk to directly.
- When you send a packet to a computer in your subnet, you put the computer's MAC address on it. TO get the right MAC, computer uses a protocol called ARP.
- `arp -na` to see the contents of the ARP table
- Routers use a protocol called **BGP** to decide what router the packet should go next.
- To describe groups of IP addresses use CIDR notation.
- In CIDR notation, a /n gives you 2<sup>32-n</sup> IP addresses.
- 10.9.0.0/24 is all the IP addresses which have the same first 24 bits as 10.9.0.0!
- To see the certificate for `jvns.ca` run `openssl s_client -connect jvns.ca:443 -servername jvns.ca`

### Transfer file using netcat

- I run: `netcat -l -p 12345 > file.pdf`
- You run: `netcat $MY_IP_ADDRESS 12345 < file.pdf`


### ARP cache poisoning : How to steal packets on a wireless network!

- `sudo arpspoof -i wlan0 -t 192.168.0.13 192.168.0.1 ` `192.168.0.13` is my phone's IP address on the local network. `192.168.0.1` is the address of the router.
- What the above line of code basically does is "Hey phone! You want to send a packet to the router? Send that to me instead. Thanks!"
- This exploitation technique is called **ARP cache poisoning**.

### Implementing traceroute

- [Scapy](https://scapy.net/) is Python networking library which lets you construct packets really easily.
- IP packets have a `ttl` attribute. Every time a machine receives an IP packet, it decreases the `ttl` by 1 and passes it on.
- If a packet's `ttl` runs out before it replies, the last machine sends back an ICMP packet saying "sorry, failed!"
- There's also `tcptraceroute` which sends TCP packets.

### Spytools

- **netstat** tells what ports are open computer : `netstat -tulpn`
- **dstat** tells how much data is actually being written to the physical hard drive right this second.
- **lsof** tells which files every process has open right now.
- [**ngrep**](https://dl.packetstormsecurity.net/papers/general/ngreptut.txt) and **tcpdump**
- **ftrace**