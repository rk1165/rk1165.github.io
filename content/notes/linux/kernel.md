+++
title = 'Linux Kernel'
date = 2024-02-11

+++

### Kernel Modules

- Modules are pieces of code that can be loaded and unloaded from the kernel upon demand.
- They extend the functionality of the base kernel without having to rebuild or recompile the kernel again.
- Most of the drivers are implemented as a Linux kernel modules.
- The kernel modules have a .ko extension.
- On a normal linux system, the kernel modules will reside inside /lib/modules/<kernel_version>/kernel/directory.
- To see what modules are already loaded run **lsmod**
- `insmod` command will insert a new module into the kernel as shown: `insmod /lib/modules/<version>/kernel/fs/squashfs/squashfs.ko`
- `modinfo` will display information about a kernel module : `modinfo /lib/modules/<version>/kernel/fs/squashfs/squashfs.ko`
- `rmmod` command will remove a module from the kernel. `rmmod squashfs.ko`
- When the kernel needs a feature that is not resident in the kernel, the kernel module daemon kmod execs modprobe to load the module in.
- modprobe is passed a string in one of two forms:
- A module name like `softdog` or `ppp`
- A more generic identifier like `char-major-10-30`
- If modprobe is handed a generic identifier, it first looks for that string in /etc/modprobe.conf. If it finds an alias line like `alias char-major-10-30 softdog` if knows that the generic identifier refers to the module `softdog.ko`
- Next modprobe looks through the file `/lib/modules/<version>/modules.dep`, to see if other modules must be loaded before the requested module may be loaded.
- Lastly, modprobe uses `insmod` to first load any prerequisite modules into the kernel, and then the requested module. modprobe directs insmod to `/lib/modules/<version>`, the standard directory for modules.

### What does the Linux kernel even do?

- memory management (RAM)
- device drivers (keyboard, network, graphics card, mouse, monitors, wireless cards, etc.)
- starting processes
- thread scheduling
- filesystems (ext3, ext4, reiserfs, fat32, etc)
- VFS: interface that lest you get files not matter what filesystem you're using
- UNIX APIs (system calls)
- POSIX security model (permissions)
- virtual machines, containers (like LXC)
- networking (bridging, firewalls, protocol implementations like TCP/IP, UDP, ethernet, ICMP, RPC, wireless)
- IPC (interprocess communication)
- signals (SIGINT, SIGKILL)
- interrupt handlers - handles events from the hardware (packet received, keypress, timers, graphics card ready, data ready, hard drive finished reading). Hardware is sometimes handled in other ways like DMA
- Timers (when I call `sleep()`)
- Timekeeping (when I ask for the time)
- architecture-specific stuff (amd64, powerpc, x86, MIPS, ARM)
- power management
- loading kernel modules
- kernel debugging tools

### What is _address space_?

- Processes each have their own PID and _address space_, threads share the PID and address space of a single process.
- Each process has a piece of memory that it's allowed to use, so that processes can't step on each others' toes. This includes
  - the _code_ or _text_ of the program
  - the program's _data_ (strings and constants)
  - the _heap_ (grows dynamically, where the memory allocated using `malloc` lives)
  - the stack (a fixed size, where local variables and functions calls live. Relevant terms: _stack overflow_, _stack trace_)
  - the environment variables
  - the command line arguments

### Why threads?

- Good things:
  - They can communicate more easily between each other, because they can just write to the same memory
  - Don't need to make a copy of the address space for each new thread
- Bad things
  - Because they write to the same memory, you can have all kinds of race conditions. So you need to use mutexes and things.

### Rootkit

- Rootkit is a kernel module that once put in a kernel, any unprivileged user who knows the right incantation can become root.
