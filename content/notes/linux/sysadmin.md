+++
title = 'System Administration'
date = 2024-02-11

+++

### Power Trip

- [This is how password cracking is done](https://null-byte.wonderhowto.com/how-to/crack-shadow-hashes-after-getting-root-linux-system-0186386/)
- In `/var/log/auth.log` any use of `sudo` is logged.
- Renaming a server is done by editing two files : `/etc/hostname` and `/etc/hosts/`
- `timedatectl` to check the current time zone.
- `timedatectl list-timezones`
- `timedatectl set-timezone Asia/Kolkata`

### Install Software, explore File System

- `man hier` to read about the hierarchy
- Most key configuration files are kept under `/etc` and subdirs of that.
- `/etc/passwd`, `/etc/ssh/sshd_config`, `/var/log/auth.log`
- Read [Repositories - CommandLine](https://help.ubuntu.com/community/Repositories/CommandLine)
- [Ubuntu and Red Hat/Cent OS package management comparison](https://help.ubuntu.com/community/SwitchingToUbuntu/FromLinux/RedHatEnterpriseLinuxAndFedora)
- [Ubuntu Server Guide - Package Management](https://ubuntu.com/server/docs/package-management)

### Ports, open and closed

- `ss` - "socket status", is a standard utility replacing `netstat`
- `ss -ltp` which ports are open on which interfaces
- The Linux kernel has built-in firewall functionality called **netfilter**. We configure and query this via various utilities, the most low-level of which are the `iptables` command, and the newer `nftables`
- `ufw` : the uncomplicated firewall
- `sudo iptables -L` : list what rules are in place
- `sudo ufw allow ssh`
- `sudo ufw deny http`
- and then `sudo ufw enable` to enable firewalls
- In practice, ensuring that you're not running unnecessary services is often enough protection, and a host-based firewall is unnecessary, but this very much depends on the type of server you are configuring
- Occasionally it may be reasonable to re-configure a service so that it’s provided on a non-standard port - this is particularly common advice for ssh/22. Experts call this “security by obscurity”
- [How to Log Linux IPTables Firewall Dropped Packets to a Log File](http://www.thegeekstuff.com/2012/08/iptables-log-packets/)
- [Firewalling with iptables - One approach](http://www.pettingers.org/code/firewall.html)
- [UFW](https://help.ubuntu.com/community/UFW)
- [Collection of basic Linux Firewall iptables](http://linuxconfig.org/collection-of-basic-linux-firewall-iptables-rules)
- [How to install nftables in Ubuntu](https://www.liquidweb.com/kb/how-to-install-nftables-in-ubuntu/)

### Running Scheduled Tasks

- list out your user crontab entry with `crontab -l` and then that for root with `sudo crontab -l`.
- there’s also a system-wide crontab defined in `/etc/crontab`
- `/etc/cron.*` folders to see what's actually scheduled, daily, weekly and monthly.
- `systemd` can _also_ be used to run tasks at specific times via **timers**.
- `systemctl list-timers`
- [Job Scheduling with "cron" and "at"](http://www.ibm.com/developerworks/linux/library/l-job-scheduling/index.html)
- [A good overview of systemd/Timers](https://wiki.archlinux.org/index.php/Systemd/Timers)
- [How to use Systemd Timers as a Cron Replacement](https://www.maketecheasier.com/use-systemd-timers-as-cron-replacement/)

### Copying with SFTP

- There is a wide range of ways a Linux server can share files, including:
  - SMB : Microsoft's file sharing, useful on a local network of Windows machines
  - AFP: Apple’s file sharing, useful on a local network of Apple machines
  - WebDAV: Sharing over web (http) protocols
  - FTP
  - scp : simple support for copying files
  - rsync : Fast, very efficient file copying
  - SFTP : File access and copying over the SSH protocol
- [CyberDuck](http://cyberduck.io/)

### ACLs

- ACLs are a second level of discretionary permissions, that may override the standard ugo/rwx ones. When used correctly they can grant a better granularity in setting access to a file or a directory.
- `tune2fs -l /filesystem` to check if it has been loaded with `acl` option.
- If it isn't we can use `mount -o remount -o acl /filesystem` (not permanent). To make it permanent edit `/etc/fstab`
- [How to manage ACLs on Linux](https://linuxconfig.org/how-to-manage-acls-on-linux)
- [Linux Access Control Lists](https://www.redhat.com/sysadmin/linux-access-control-lists)
- [SELinux For Mere mortals](https://craigmbooth.com/blog/selinux-for-mortals/)
- [File Security](http://tldp.org/LDP/intro-linux/html/sect_03_04.html)

### sudo

- [Sudo - An advanced HowTo](https://centoshelp.org/security/sudo-an-advanced-howto/)

### Deeper into repositories...

- to see where the packages you install are coming from, view `/etc/apt/sources.list` where you'll see lines that are clearly specifying URLs to a “repository” for your specific version
- to see how many packages you could already install : `apt-cache dump`
- Multiverse: "contains software which has been classified as non-free ...may not include security updates".
- Examples of useful tools in Multiverse might include the compression utilities `rar` and `lha`, and the network performance tool `netperf`
- To enable the Multiverse repository, follow the guides at:
  - [Repositories/Ubuntu](https://help.ubuntu.com/community/Repositories/Ubuntu)
  - [Ubuntu Server Guide](https://help.ubuntu.com/lts/serverguide/configuration.html)
- [Package Management with APT](https://help.ubuntu.com/community/AptGet/Howto)
- [How to use yum](http://fedoranews.org/tchung/howto/2003-11-09-yum-intro.shtml)
- [What do you mean by Free Software?](http://www.debian.org/intro/free)

### From the Source code

- `./configure` : is a script which checks to see whether the server is Intel or ARM and other stuff.
- `make` - compiles the software
- `make install` : takes the compiled files, and installs that into the system. In some cases also sets up services and scheduled tasks etc.
- [What is Linux From Scratch](http://www.linuxfromscratch.org/lfs/)
- [The Arch Build System](https://wiki.archlinux.org/index.php/Arch_Build_System)
- [Installing from tarballs](http://linux.byexamples.com/archives/156/installing-from-tarballs/)
- [How to rebuild an exsisting package from source](http://raphaelhertzog.com/2010/12/15/howto-to-rebuild-debian-packages/)
- [Compiling things on Ubuntu the Easy way](https://help.ubuntu.com/community/CompilingEasyHowTo)

### Log rotation

- Log rotation is an automated process used in which log files are compressed, moved, renamed or deleted once they are too old or too big.
- Using `logrotate` you can define how many days of logs you wish to keep; split them into manageable files; compress them to save space, or even keep them on a totally separate server.
- The overall configuration is set in `/etc/logrotate.conf`.
- Also look at the files under the directory `/etc/logrotate.d`, as the contents of these are merged in to create the full configuration.
- [The Ultimate Logrotate Tutorial](http://www.thegeekstuff.com/2010/07/logrotate-examples/)
- [Use logrotate to Manage Log Files](http://library.linode.com/linux-tools/utilities/logrotate)

### Inodes, symlinks and stat

- Linux has an extra layer between the filename and the file's actual data on the disk - this is the _inode_
- `ls -li /etc/hosts` to see the inode.
- `stat /etc/hosts`
- When we view the permissions, ownership and dates of filenames, these attributes are actually kept at the inode level, _not_ the filename.
- `/etc/rc2.d/*` holds all the scripts that start when your machine changes to “runlevel 2” (it's normal running state)
- [Anatomy of the Linux file system](https://developer.ibm.com/tutorials/l-linux-filesystem/)
- [Hard and soft links](http://linuxgazette.net/105/pitcher.html)
- [What's an inode](http://www.linux-mag.com/id/8658/)
- [Unix filesystem inodes](http://www.cyberciti.biz/tips/understanding-unixlinux-filesystem-inodes.html)

### Scripting

- If we want an executable to be run directly without giving the full path here are a couple of ways how we can do that.
  - We can copy the script to one of the directory which is present in the PATH variable so that the system picks it up.
  - We can create a directory and keep our executables there and add that directory in the PATH.
- [Bash scripting tutorial](http://linuxconfig.org/Bash_scripting_Tutorial)
- [BASH Programming](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)

### Ip Addressing

- An IP address is divided into two main sections: The network part and the host part. 
- In a simple IP address of 192.168.1.5 with a subnet mask or netmask of 255.255.255.0, the first three octets from the left represent the network part, and the remaining octet is the portion that is assigned to host machines on your network.
- The purpose of a subnet is to draw a boundary between the network portion of an IP address and the host portion. For each bit of the IP address, the subnet or netmask assigns a value.
- For the network portion, it turns on the bit and assigns the value of 1, For the host portion, it turns off the bit and assigns the value of 0
- A commonly used subnet mask is the Class C subnet which is 255.255.255.0.
- There are 3 main classes of IP addresses which can be organized in the table below

| Class | 1st octet | Default Subnet mask | Network/Host | # networks | max nodes in network |
| ----- | --------- | ------------------- | ------------ | ---------- | -------------------- |
| A     | 1 - 127   | 255.0.0.0           | N.H.H.H      | 126        | 16,777, 214          |
| B     | 128 - 191 | 255.255.0.0         | N.N.H.H      | 16,384     | 65,534               |
| C     | 192 - 223 | 255.255.255.0       | N.N.N.H      | 2,097,152  | 254                  |
| D     | 224 - 239 |                     |              |            |                      |
| E     | 240 - 254 |                     |              |            |                      |

- All IPv4 addresses can also be categorized as either Public or Private IP addresses.
- Private IP addresses are assigned to hosts with a LAN. Below is a range of Private IP addresses
  - 10.0.0.0 - 10.255.255.255
  - 172.16.0.0 - 172.31.255.255
  - 192.168.0.0 - 192.168.255.255
- Anything outside this range is a public IP address. Public IP addresses are assigned over the internet by ISPs.
- Public IP is then mapped to a private IP addresses in your LAN with the help of NAT.
- `ip link show`
- `sudo ip link set enp0s3 up` bring the interface up
- `ip neighbor show` : to check ARP table

### Sticky bit, SUID, SGID

- A sticky bit is a special file permission set on a file or entire directory.
- It grants only the owner of that file/directory the permission to delete or make changes to the file or directory contents.
- It has symbolic value of `t` and numeric value of `1000`
- `chmod +t directory_name` : All the contents will inherit the sticky bit permissions.
- SUID (Set User ID) is another special file permission that allows another regular user to run a file with the file permissions of the file owner.
- It is usually denoted by a symbolic `s` at the user's portion of file permissions instead of an `x`. It has a numeric value of `4000`
- SGID (Set Group ID) allows a regular user to inherit the group permissions of the file group owner.
- Rather than the `x` for execute permissions, you will see an `s` in the group portion of the file permissions.
- SGID has numeric value of `2000`
- SUID and SGID should be avoided at all costs.
- `chmod u+s executable` : add the **Set User ID** capability for the **u**ser who owns the file to `executable`. It is a way to give others the same permissions as the owner when others execute a program which requires reading a file whose access they don't have. It temporarily changes other's ID to owner's Id.
