+++
title = 'Networking Tools'
date = 2024-02-11

+++

### dig

- dig is a flexibe tool for interrogating DNS name servers.
- It also has a batch mode of operation for reading lookup requests from a file.
- A typical invocation of dig looks like: `dig @server name type` where:
  - `server`: is the name or IP address of the name server to query.
  - `name` is the name of the resource record that is to be looked up
  - `type` indicates what type of query is required - ANY, A, MX, SIG, etc. If no type argument is supplied : dig performs a lookup for an A record.
- `dig jvns.ca`
- `dig @8.8.8.8 jvns.ca`
- `dig +trace jvns.ca` does the same thing a recursive DNS server would do to find the domain's IP
- `dig +short @127.0.0.11 rails_server` ($name of the container/service_from_dcompose)
- `dig +short @127.0.0.11 db`
- `dig devopswork.com @8.8.8.8 NS`
- `dig devopswork.com NS`
- `dig www.google.com @8.8.8.8 +trace`
- `dig www.google.com +trace`
- `dig www.google.com @8.8.8.8 +short`
- `dig www.google.com @8.8.8.8`
- `dig www.google.com`

### ftp

- `ftp hostname`
- `ftp://username:password@hostname`
- `cd folder`
- `get file`
- `put file`
- `mget filenameregx`
- `mput filenameregx`
- The only trick is sometimes you might need to put it into binary mode by typing binary. (where to type binary)
- To exit, type `bye`

### nslookup

- `nslookup` is a program to query Internet DNS.
- `nslookup` has two modes: interactive and non-interactive.
- Interactive mode allows the user to query name servers for information about various hosts and domains or to print a list of hosts in a domain.
- Non-interactive mode is used to print just the name and requested information for a host or domain.
- it has its own set of dials and knobs, called _option settings_.
- The options come in two flavors: Boolean and value. The options that do not have an equals sign after them are boolean options.
- Options to be set:
  - `nodebug` `defname` `search` `recurse` `nod2` `novc` `noignoretc`
  - `port=53 querytype=A class=IN timeout=5 retry=4`
  - `root=a.root-servers.net. domain=fx.movie.edu srchlist=fx.movie.edu`
- In an interactive session, you change an option with the set command, as in `set debug`
- From the command line, you omit the word set and precede the option with a hyphen, as in `nslookup -debug`
- Chores for nslookup : finding the IP address or MX records for a given domain name, or querying a particular name server for data.
- By default, nslookup looks up the address for a domain name, or the domain name for an address
- You can switch servers with nslookup by using the `server` or `lserver` command.
- `lserver` queries local name server to get the address of the server you want to switch to.
- To specify that you'd like nslookup to query a particular name server for information about a given domain name, you can specify the server as the second argument on the line, after the domain name to look up

### traceroute

- traceroute can be used to find the sequence of routers through which a message is routed.

### whois

- WHOIS (pronounced as the phrase "who is") is a query and response protocol that is widely used for querying databases that store the registered users or assignees of an Internet resource, such as a domain name, an IP address block or an autonomous system, but is also used for a wider range of other information.
- `whois $IP_ADDR` gives the range of IP addresses used and a lot of other details.
- `whois -h whois.arin.net "flag search-term"` The parts of this command are:
  - whois : the command itself
  - -h : specifies that the hostname of the Whois server will follow
  - whois.arin.net: the name of ARIN's Whois server
  - flag: narrows the search by restricting the results to those that match criteria designated by the flag.
  - search-term: the information for which you are searching

### netstat

- `netstat -a`  : list all ports
- `netstat -at` : list all tcp ports
- `netstat -au` : list all udp ports
- `netstat -l`  : list only listening ports
- `netstat -lt` : lost only listening TCP ports
- `netstat -lu` : list only listenig UDP ports
- `netstat -lx` : list only the listening UNIX ports
- `netstat -s`  : show statistics for all ports
- `netstat -st` : show stats for TCP ports
- `netstat -su` : show stats for UDP ports
- `netstat -pt` : adds the PID/program name
- `netstat -an` : don't resolve host, port and user name in netstat o/p
- `netstat -c`  : print information continuously every few seconds
- `netstat -r`  : display the kernel routing information
- `netstat -i`  : show the list of network interfaces
- `netstat -rn` : to display routes in numeric format w/o resolving for hostnames
- `netstat -ie` : display extended information on the interfaces
- `netstat --verbose`            : find the non supportive address families in your system
- `netstat -ap | grep ssh`       : find out on which port a program is running
- `netstat -an | grep ':80'`     : find out which process is using a particular port
- `netstat -tulpn | grep LISTEN` : to see which ports are listening.

### ss

- `ss` : lists all the connections regardless of the state they are in
- `ss -a`  : retrieve a list of both listening and non-listening ports
- `ss -l`  : display listening sockets only
- `ss -t`  : display all TCP connections
- `ss -lt` : view all the listening TCP socket connection
- `ss -ua` : view all the UDP socket connections
- `ss -lu` : view all listening UDP connections
- `ss -p`  : display the PIDs related to socket connections
- `ss -s`  : display summary statistics
- `ss -4`  : display the IPv4 socket connections
- `ss -6`  : display the IPv6 socket connections
- `ss -at '( dport = :22 or sport = :22 )'` : filter socket port number or address number. Here it displays all socket connections with a destination or source port of ssh.

### ufw

- `ufw` : default firewall in ubuntu.
- `sudo ufw default deny incoming`
- `sudo ufw default allow outgoing`
- `sudo ufw allow ssh`
- `sudo ufw allow www` : allowing http
- `sudo ufw enable`

### scp

- `scp mailout.jar c5017417@server_name:/tmp` securely copy `mailout.jar` to `server_name`'s `/tmp` directory

### netcat

- `nc -zv www.google.com 80`
