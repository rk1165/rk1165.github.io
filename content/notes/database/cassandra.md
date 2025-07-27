+++
title = 'Cassandra'
date = 2024-02-11

+++

### Architecture

- gossip protocol is primarily implemented by the `org.apache.cassandra.gms.Gossiper` class, which is responsible for managing gossip for the local node.
- Failure detection is implemented in Cassandra by the `org.apache.cassandra.gms.FailureDetector` class
  - `isAlive` : what the detector will report about a given node's alive-ness
  - inte`rpret : used by the gossiper to decide whether a node is alive or not based on suspicion level
  - `report` : when a node receives a heartbeat, it invokes this method
- The snitches can be found in the package `org.apache.cassandra.locator`. Each snitch implements the `IEndpointSnitch` interface.

### Rules of a Good partition

- Store together what you retrieve together
- Avoid big and constantly growing partitions
  - Up to 2 billion cells per partition
  - Up to ~100k rows in a partition
  - Up to ~100MB in a partition
  - `PRIMARY KEY ((sensor_id, month_year), reported_at);` BUCKETING
  - UDTs cannot be used in primary key
- Avoid hot partitions : a partition that is get hit a lot more
