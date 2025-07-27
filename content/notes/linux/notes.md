+++
title = 'Linux Notes'
date = 2024-02-11

+++

### Things to learn about Linux

- tcp/ip & networking stuff
- What is a port/socket?
- seccomp
- systemd
- IPC
- permissions, setuid, sticky bits, how does chown work
- how the shell uses fork & exec
- how can I make my computer a router?
- process groups, session leaders, shell job control.
- memory allocation, how do heaps work, what does malloc do?
- ttys, how to terminals work
- process scheduling
- the kernel, drivers, modern X servers
- How does X11 work?
- Linux's zero-copy API (sendfile, splice, tee)
- what is dmesg even doing
- How kernel modules work.
- embedded stuff : realtime, GPIO etc.
- btrfs
- QEMU/KVM
- shell redirection
- HAL
- chroot
- filesystems and inodes
- how do I know how much memory my process is using
- iptables
- what is a network interface exactly?
- what is syslog and how does it work?
- how are logs usually recognized?
- virtual memory
- BPF
- bootloader, initrd, kernel parameters.
- the `ip` command
- what are all the files that are not file files (/dev, stdin, /proc, /sys)
- dbus
- sed and awk
- namespaces, cgroups, docker, SELinux, AppArmor
- difference between threads and processes
- kpatch, kgraph, kexec
- package management
- mountfs and vfs
- System calls are how applications talk to the operating system.
- OOM killer: it's a system in the Linux kernel that starts just killing programs on your computer!
- Four reasons by a Linux box might swap:
  1. could be actually out of RAM
  2. could be "mostly" out of RAM. The "vm.swappiness" sysctl setting controls how likely your machine is to swap
  3. A cgroup could be out of RAM
  4. if you have no swap, and your vm.overcommit_ratio is set to 50% (which is the default), you can end up in a situation where only half your RAM can be used

### Linux Networking tools

| Command                   | Description                                                          |
| ------------------------- | -------------------------------------------------------------------- |
| ping                      | are these computers even connected                                   |
| curl                      | make any HTTP request you want                                       |
| httpie                    | like curl but easier ("http get")                                    |
| wget                      | download files                                                       |
| tc                        | on a linux router: slow down your brother's internet (and much more) |
| dig/nslookup              | what's the IP for that domain? (DNS query)                           |
| whois                     | is this domain registered                                            |
| ssh                       | secure shell                                                         |
| scp                       | copy files over a SSH connection                                     |
| rsync                     | copy only changed files (works over SSH)                             |
| ngrep                     | grep from your network                                               |
| tcpdump                   | show me all packets on port 80                                       |
| wireshark                 | look at packets in a GUI                                             |
| tshark                    | command line super powerful packet analysis                          |
| tcpflow                   | capture & assemble TCP streams                                       |
| ifconfig                  | what's my IP address                                                 |
| route                     | view & change the route table                                        |
| ip                        | replaces ifconfig, route, and more                                   |
| arp                       | see your ARP table                                                   |
| mitmproxy                 | spy on SSL connections your programs are making                      |
| nmap                      | in your network scanning your ports                                  |
| zenmap                    | GUI for nmap                                                         |
| pof                       | identify OS of hosts connecting to you                               |
| openvpn                   | a VPN                                                                |
| wireguard                 | a newer VPN                                                          |
| nc                        | netcat! make TCP connections manually                                |
| socat                     | proxy a TCP socket to a unix domain socket + LOTS MORE               |
| telnet                    | like ssh but insecure                                                |
| ftp/sftp                  | copy files. sftp does it over SSH                                    |
| netstat/ss/lsof/fuser     | what ports are server using                                          |
| iptables                  | set up firewalls and NAT                                             |
| nftables                  | new version of iptables                                              |
| hping3                    | construct any TCP packet you want                                    |
| traceroute/mtr            | what servers are on the way to that server                           |
| tcptraceroute             | use tcp packets instead of icmp to traceroute                        |
| ethtool                   | manage physical Ethernet connections + network cards                 |
| iw / iwconfig             | manage wireless network settings (see speed/frequency)               |
| sysctl                    | configure linux kernel's network stack                               |
| openssl                   | do literally anything with SSL certificates                          |
| stunnel                   | make a SSL proxy for an insecure server                              |
| iptraf/nethogs/iftop/ntop | see what's using bandwidth                                           |
| ab/nload/iperf            | benchmarking tools                                                   |
| python3 -m http.server    | serve files from a directory                                         |
| ipcalc                    | easily see what 13.21.2.3/25 means                                   |
| nsenter                   | enter a container process's network namespace                        |

### Random

- In `/proc/<PID>` there is a lot of very useful information about process with pid PID.
- in `/proc/PID/env` lives all of the process's environment variables.
- in `/proc/PID/fd` one will find links to all open files.
- in `/proc/PID/cmdline` is the command line arguments it was started with.
- `ls -l /proc/$PID/fd/{0,1,2}` : to see which files a process is reading or writing data to
- If you look at the file `/proc/$pid/status`, you can find out all sorts of information about your processes
- Every program has an action it does for every signal and every signal has a default action.
- SIGTERM && SIGHUP && SIGINT -> Terminate.
- SIGWINCH -> ignore (windoe resize signal)
- Can Customize how to respond to most signals but not SIGKILL. SIGKILL(kill -9) means dead.
- Linux caches "pages" (4K blocks) also known as "page cache"
- man pages are split into 8 sections
  1. **programs** : man grep, man ls
  2. **system calls** : man sendfile
  3. **C functions** : man 3 printf, man fopen
  4. **devices** : man null
  5. **file formats** : man sudoers, man proc
  6. **games**
  7. **miscellaneous** : man 7 pipe, man 7 symlink
  8. **sysadmin programs** : man apt, man chroot
- `ls -l /bin/ping` : rwsr-xr-x : **s** is `setuid` flag. This means ping always runs as root, no matter who started ping.
- Every file has an inode:
  - who owns the file?
  - when was it last changed?
  - where is the file's data?
- `ls -i` tells inode numbers.
- inodes live in a HUGE ARRAY on hard drive. One for every single file + directory.

