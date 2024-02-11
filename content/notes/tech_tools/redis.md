### Redis

- Redis is Key-Value Store NoSQL Database.
- Key is a string, value is pretty much anything.
- Redis is in memory first
- Single threaded (except when durability is enabled)
- Optional Durability
  - Journaling (append only log AOP)
  - Snapshotting
- Redis uses TCP.
- Message format is RESP (REdis Serialization Protocol)
- Replication - One Leader many followers model
- Clustering - Shard data across multiple nodes
- Redis supports pub/sub mechanism. We can publish to a channel.
