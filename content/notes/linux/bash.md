+++
title = 'Bash'
date = 2024-02-11

+++

### Bash utilities

- Every job on the system is assigned a _priority_
- `stty -a` to see the terminal settings
- `Ctrl-C` gives the running job more of a chance to clean up before exiting.
- Emacs mode commands

| command  | Description                    |
| -------- | ------------------------------ |
| Ctrl-b   | Move backward one char         |
| Ctrl-f   | Move forward one char          |
| Ctrl-d   | Delete one char forward        |
| Ctrl <-  | Move one word backward         |
| Ctrl ->  | Move one word forward          |
| ESC-bks  | Kill one word backward         |
| ESC-d    | Kill on word forward           |
| Ctrl-y   | Retrieve last item killed      |
| Ctrl-t   | Exchange chars at cursor       |
| M-t      | Exchange words at cursor       |
| M-u      | Uppercase the current word     |
| M-l      | Lowecase the current word      |
| M-c      | Capitalize the current word    |
| C-x bksp | Kill backward line             |
| C-u      | Kill backward from point       |
| M-d      | Kill from point to end of word |
| M-bksp   | Kill the word behind point     |

- `set -o vi`
- vi-mode essentially cretes a one-line editing window into the history list.
- vi control mode commands for searching the command history

| Command | Description                                     |
| ------- | ----------------------------------------------- |
| k or -  | Move backward one line                          |
| j or +  | Move forward one line                           |
| G       | Move to line given by repeat count              |
| /string | Search backward for string                      |
| ?string | Search forward for string                       |
| n       | Repeat search in same direction as previous     |
| N       | Repeat search in opposite direction as previous |

- vi-mode character finding commands

| command | Description                                                   |
| ------- | ------------------------------------------------------------- |
| fx      | Move right to next occurrence of x                            |
| Fx      | Move left to previous occurrence of x                         |
| tx      | Move right of next occurrence of x, then back one space       |
| Tx      | Move left to previous occurrence of x, then forward one space |
| ;       | Redo last character-finding command                           |
| ,       | Redo last character-finding command in opposite direction     |

- `\` is the command that tells bash to do completion in vi-mode.
- the `-l` option to `fc` lists previous commands.
- `bind` command can be used to bind key bindings
- `bind -P` to print out the bindings currently in effect
- `bind -l` to see the names of the readline functions
- `bind -u` followed by name of the function to unbind a function.
- `bind -r` followed by sequence to unbind a key sequence
- `bind -x` to bind a shell command to a key sequence. eg. bind -x `'\C-l":ls'`
- `bind -p` prints out the bindings to std output in a format that can be re-read by bind or used as a .inputrc file.
- `bind -f` takes a filename and reads the key bindings from that file.

### Writing a disc image to a USB device

1. Connect the USB flash drive to your PC, open a terminal and cd to where you downloaded the disc image.
2. Check the device identifier with: `sudo fdisk -l`.
3. Write the image with: `sudo dd if=manjaro-xfce-17.1-stable-x86_64.iso of=/dev/(Device identifier from above) bs=4M`

### User management

- To list users currently logged on the system, the `who` command can be used
- To list all existing users accounts including their properties stored in the user database, run `passwd -Sa` as root
- To add a new user `useradd -m -g intial_group -G additional_groups -s login_shell username`
- `sudo groupadd editorial` : create a group editorial
- `sudo usermod -a -G editorial user` : adds the user to group editorial. `-a` tells we are appending, and `-G` tells we are appending to the group name that follows the option.
- we can use `members group_name` to find which users are in a group
- Information about users in a Linux system is stored in the following files:
  - /etc/passwd : contanins info about users in various fields.
  - /etc/group : contains info about the user groups. `groups username` to see what groups a user belongs to
  - /etc/gshadow : contains encrypted or **shadowed** passwords for group accounts and, for security reasons, cannot be accessed by regular users.
  - /etc/shadow : stores the users actual passwords in a hashed or encrypted format.
- `adduser username` : for adding users
- `passwd -l username` : lock the user from loggin into the system
- `deluser --remove-home bob`
- The root user can add a user to an existing group with the command: `usermod -a -G group user`

### IMTx

- /sbin : binaries for root user
- /etc: editable text configurations
- /lib:
- /var: databases, logs
- /usr : unix system resources. extend the system operations.
- printf() is buffer. if not using "\n" the printf() is displayed in the last. as a remedy we use fflush(stdout) when asking input from the user.
- Instead of scanf we should use **fgets** (which gets all the character even the "\n") also we can use **getline** (compatible with POSIX) and also **readline** (only compatible in linux)

### How to use grep on all files non-recursively in a directory?

- need to specify hidden files ._ and non-hidden _
- To avoid "ls a directory" errors use `-d skip`
- using `-s` hides all the error message
- `grep -s "string * .*"`

### Services

- A Linux systems provide a variety of system services (such as process management, login, syslog, cron, etc.) and network services (such as remote login, e-mail, printers, web hosting, data storage, file transfer, domain name resolution (using DNS), dynamic IP address assignment (using DHCP), and much more).
- Technically, a service is a process or group of processes (commonly known as daemons) running continuously in the background, waiting for requests to come in (especially from clients).
- `systemctl list-units --type=service` or `systemctl --type=service` to list all loaded services (whether active, running, exited, or failed)
- `systemctl list-units --type=service --state=active` or `systemctl --type=service --state=active` to list all loaded but active services, both running and those that have exited.
- `systemctl list-units --type=service --state=running` or `systemctl --type=service --state=running` to list all loaded and actively running.

### Questions

- How do I remove the first 300 million lines from a 700 GB txt file on a system with 1 TB max disk space?
  - If you have enough space to compress the file, which should free a significant amount of space, allowing you to do other operations, you can try this:
    `gzip file && zcat file.gz | tail -n +300000001 | gzip > newFile.gz`
  - The `&&` operation ensures that you only continue if the `gzip` operations was successful.
- Finding the PID of the process using a specific port
  - `sudo ss -lptn 'sport = :80'`
  - `sudo netstat -nlp | grep :80`
  - `sudo lsof -n -i :80 | grep LISTEN`
- zip -sf test.zip

- How to transponse a file like this

```bash
name age
alice 21
ryan 30
```

- Two ways:
- `head -1 file.txt | wc -w | xargs seq 1 | xargs -I{} -n 1 sh -c "cut -d ' ' -f{} file.txt | paste -sd ' ' -"`

```bash
awk '
{
    for (i = 1; i <= NF; i++) {
        if(NR == 1) {
            s[i] = $i;
        } else {
            s[i] = s[i] " " $i;
        }
    }
}
END {
    for (i = 1; s[i] != ""; i++) {
        print s[i];
    }
}' file.txt
```

- With the use of **!$** You can recall last argument of the precious command, so it will now look like that.

### Commands to collect system and hardware information

- uname -n/-v/-r/-m/-a
- `sudo lshw` : to view linux system hardware information
- `sudo lshw -short` : to print a summary of hardware information
- `sudo lshw -html > lshw.html` : to generate the o/p as html file
- `lscpu` : to view information about CPU.
- Block devices are storage devices such as hard disks, flash drives, etc.
- `lsblk` : to view information about block devices
- `lsblk -a` : to view information about all block devices
- `lsusb` : to view information about USB controllers and all the devices connected to them
- `lsusb -v` : to view USB information in verbose form
- PCI devices may include usb ports, graphics cards, network adapters etc.
- `lspci` : to view information concerting all PCI controllers and the devices connected to them
- `lspci -v` : to view in verbose form
- `sudo fdisk -l` : to gather info about file system partitions.
- you can use the [dmidecode utility](https://www.tecmint.com/how-to-get-hardware-information-with-dmidecode-command-on-linux/) to extract hardware information by reading data from the DMI tables.
- `sudo dmidecode -t memory`    : to print information about memory
- `sudo dmidecode -t system`    : to print information about system
- `sudo dmidecode -t bios`      : to print information about bios
- `sudo dmidecode -t processor` : to print information about processor

### Useful and not much known linux commands

- `sudo !!` : redo the last command but as root
- `ctrl + x + e` : open an editor to run a command
- `fc` : fix or change a really long command that you last ran with an editor
- `ssh -L 8080:127.0.0.1:6070 root@my-public-server.com -N` : create a tunnel with SSH to port that is not open to the public (local port 8080 -> remote host’s 127.0.0.1 on port 6070)
- `cat somefile | tee -a log.txt | cat > /dev/null` : log output but don't show it on the console
- `disown -a && exit` : exit terminal but leave all processes running
- `alt + -` : paste the arguments of the previous command
- `reset` : completely clean and clear you terminal
- `grep -rn /etc/ -e text_to_find` : recursively find a text in a directory that have lot of files in it.
- `curl ifconfig.me` : find your public IP
- `!$` : use the last command's last argument
- `!*` : use the last command's all arguments
- `wget --spider --force-html -r -l5 https://<DOMAIN-NAME> 2>&1 | grep '^--' | awk '{print $3}' > urls.txt` : very quickly spider a website and generate a text file of all the URLs that it finds
- `wget --mirror --page-requisites --html-extension --convert-links <SITE-ADDRESS>` : All you need to do is to replace with the web address of the website (without HTTP/HTTPS) that you use to access the site e.g. www.example.com.
  - This command will mirror the target website into a directory under the one you are in with the name of the domain name.

### Commands to quickly review Linux Server Usage

- `vmstat 1` : This command will tell you information about RAM and disk unsage. Take a look to review the amount of data going into disk and RAM. The **iowait** stat also tells you if disk contention is causing speed issues.
- `mpstat -P ALL 1` : This will give you an idea of the work balance across the CPU cores.
- `pidstat 1` : This will tell you what processes and users are using the CPU resources.
- `iostat 1` : This tells you the disk I/0
- `free -m` : This will tell you the current memory usage and available free memory. The `buff/cache` and `available` lines will tell you how much memory your server has available.
- `sar -n DEV 1` : This will tell you how much data is being received and transmitted from your server.

### Shell script best practices

- Use `bash`.
- Make the first line be `#!/usr/bin/env bash`
- Use `set -o errexit` so that when a command fails, `bash` exits instead of continuing with the rest of the script.
- Prefer to use `set -o nounset`. This will make the script fail, when accessing an unset variable.
  - When you want to access a variable that may or may not have been set, use `"${VARNAME-}"` instead of `"$VARNAME"`
- Use `set -o pipefail`. This will ensure that a pipeline command is treated as failed, even if one command in pipeline fails.
- Use `set -o xtrace`, with a check on `$TRACE` env variable. This helps in debugging. People can _enable_ debug mode, by running your script as `TRACE=1 ./script.sh`
- Use `[[ ]]` for conditions in `if/while` statements
- When printing error messages, redirect to stderr. Use `echo 'Something unexptected happened' >&2`
- Use `cd "$(dirname "$0")"` to change to script's directory close to the start of the script.
- `declare -a` : the variable is an array of strings
- `declare -A` : the variable is an associative array of strings
- `declare -i` : the variable holds an integer
- `declare -r` : the variable can no longer be modified
- `declare -x` : the variable is marked for export meaning it will be imported by an child process
- `shopt -s extglob` : to toggle extended globs
- **?(list)** : Matches zero or one occurrence of the given patterns
- **\*(list)** : Matches zero or more
- **+(list)** : Matches one or more
- **@(list)** : Matches one of the given patterns
- **!(list)** : Matches anything but the given patterns
- Tests supported by `[` (also known as `test`) and `[[`
  - **-e FILE** : True if file exists
  - **-f FILE** : True if file is a regular file
  - **-d FILE** : True if file is a directory
  - **-h FILE** : True if file is a symbolic link
  - **-p PIPE** : True if pipe exists
  - **-r FILE** : True if the file is readable by you
  - **-s FILE** : True if file exists and is not empty
  - **-t FD**   : True if FD is opened on a terminal
  - **-w FILE** : True if the file is writable by you
  - **-x FILE** : True if the file is executable by you
  - **-O FILE** : True if the file is effectively owned by you
  - **-G FILE** : True if the file is effectively owned by your group
  - **FILE -nt FILE** : True if the first file is newer than the second
  - **FILE -ot FILE** : True if the first file is older than the second
  - **-z STRING** : True if the string is empty
  - **-n STRING** : True if the string is not empty
- Additional tests supported only by `[[`
  - **STRING = (or ==) PATTERN** : not string comparison but pattern matching is performed
  - **STRING != PATTERN** : Not string comparison but pattern matching is performed
  - **STRING =~ REGEX** : True if the string matches the regex pattern
  - **( EXPR )** : Parentheses can be used to change the evaluation precedence
  - **EXPR && EXPR** : doesn't evaulate the second expression if the first already turns out ot be false
  - **EXPR || EXPR** : doesn't evaluate the second expression if the first already turns out to be true
- Using `;&` instead of `;;` will grant you the ability to fall-through the case matching in bash, zsh and ksh

