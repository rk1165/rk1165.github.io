+++
title = 'Building a Distributed Snowflake Id Generator'
date = 2026-01-09T10:40:41+05:30
# categories = ["java", "system design"]
# tags = ["distributed systems", "howto"]

+++


Snowflake ID generator consists of 64 bits
- 1st bit is unused and kept as 0
- Next 41 bits make up the timestamp section
- Next 5 bits make up datacenter ID
- Next 5 bits make up the machine ID
- Last 12 bites make up the sequence number.
- For our use case we will combine datacenter and machineID and tag them as nodeId.

- Suppose we are receiving very high requests
  At any point of time state will have [T1 | S1]
  we extract them using bitwise operators
  we stored currentTimeStamp in the beginning
  if the lastTimeStamp is same as currentTimeStamp means we are generating an id in the same milliseconds so we just generate the next sequence which is supposed to be between 2^12 (0-4095)
  if we generated more than 4096 ids within a ms (T2) which means after the last id the sequence wraps around and becomes 0, we wait until the currentTime (T2) is less than or equal to itself.
  otherwise newTimeStamp becomes currentTimeStamp which might be T2 itself or move to next T3 at which point we will reset the sequence to 0
  then we pack the newTimeStamp i.e the millisecond during which the ids get generated and the sequence which got generated into newState. We do a compareAndSet operation on the state and return the snowflakeid back. In case the compareAndSet operation fails we will retry since the loop is infinite.

There are multiple nodes which can be generating the ids.
Within a node the generated Ids are sequential.
We are using mysql to acquire a node
