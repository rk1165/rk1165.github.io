+++
title = 'Peer to Peer'
date = 2024-02-11

+++

### A BitTorrent file distribution consists of these entities:

- Bit Torrent advantage over plain HTTP is that when multiple downloads of the same file happen concurrently, the downloaders upload to each other.
- An Ordinary web server
- A static 'metainfo' file
- A BitTorrent tracker
- An 'original' downloader
- The end user web browsers
- The end user downloaders
- DHTs are the foundation of current P2P networks.
- Features of the implemented P2P network:
  - DHT
  - File Sharding
  - Nodes that act as both clients and servers
- Different DHT modes : Chord, Pastry, Apache Casssandra, Kademlia
- Concepts involved : nodes, routing tables, buckets, xor distance, routing algorithms, RPC...
- **Kademlia** : A Kademlia network consists of many nodes.
- Each node:
  - has a unique 160 bit ID
  - Maintains a routing table containing contact information for other nodes.
  - Maintains its own segment of the larger DHT
  - Communicates with other nodes via 4 remote procedure calls.
- Each node's routing table is divided into 'buckets' - each bucket contains information for nodes of a specific 'distance' from the current node. The distance is the 'shared bit length' calculated using the XOR of contact's ID with the current node's ID.
- Each contact contains the ID, IP address and port number ofthe other node.
- Because Xorro is a file sharing application, the DHT segment will contain Key/Value pairs where each key is a file_id and the corresponding value is the file location.

#### Node Communications

- Four primitive remote procedure calls that Kademlia nodes send and respond to.
- **PING**: To verify that another node is still alive.
- **FIND NODE** : This RPC is sent with a specific node ID to be found. The recipient of this RPC looks in its own routing table and returns a set of contacts that are closest to the ID that is being looked up.
- **FIND VALUE** : This RPC is sent with a specific file ID to be located. If the receiving node finds this ID in tis own DHT segment, it will return the corresponding value (URL). If not, the recipient node returns a list of contacts that are closes to the file ID.
- **STORE** : Used to store a Key/Value pair in the DHT segment of the recipient node.
- Closeness is calculated using XOR of contact's ID and other node's ID.
- More shared bit pre-fix = closer distance.
- Each node in Kademlia can be thought of as a leaf in a binary tree.

#### File Sharding and Retrieval

- File sharding is the process of chopping up a file into smaller pieces, and documenting the files/names in a manifest file such that they can be retrieved and reassembled in the appropriate order.
- Sharding improves distribution and reliability of a P2P network, as multiple nodes can store some or all shards of a shared file.
- If a node containing download information for a shard goes offline, the shard info can be retrieved from a different source.

#### Resources

- [Kademlia Whitepaper, Maymounkov & Maziéres](https://pdos.csail.mit.edu/~petar/papers/maymounkov-kademlia-lncs.pdf)
- [Kademlia Whitepaper, Pita](http://maude.sip.ucm.es/kademlia/files/pita_kademlia.pdf)
- [BitTorrent DHT Protocol](http://www.bittorrent.org/beps/bep_0005.html)
- [Kademlia Design Specification, XLattice/Sourceforge](http://xlattice.sourceforge.net/components/protocol/kademlia/specs.html)
- [LibTorrent Kademlia DHT Walkthrough, Gubatron](https://gist.github.com/gubatron/cd9cfa66839e18e49846)
- [Implementing a DHT in Go, Johnson](http://blog.notdot.net/2009/11/Implementing-a-DHT-in-Go-part-1)
- [BitTorrent Tech Talks: DHT](http://engineering.bittorrent.com/tag/kademlia/)
