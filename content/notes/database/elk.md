+++
title = 'ELK Stack'
date = 2024-02-11

+++

### Components

- Kibana is an analytics & visualization platform. It is an ES dashboard where we can create pie-charts, graph etc.
- Traditionally logstash has been used to process logs and send them to ES. It's a data processing pipeline.
- Input Plugins -> Filter Plugins -> Output plugins (stashes)
- X-pack adds additional features to ES and Kibana such as Security, Monitoring, Alerting, Reporting and also enables Kibana to do ML, Graph, ES SQL
- Data ingestion - Logstash and Beats -> Search, analyze and store data : ES -> visualize your data : Kibana
- Configure MetricBeat to send data to ES
- Kibana config is stored within ES
- elasticsearch-plugin : used to install plugin
- elasticsearch-sql-cli : used to run ES SQL queries.
- `cluster.name` : must be set as best practice
- `node.name` : must be set as best practice
- `path.data`, `path.logs` : default in elastic search directory, but we should store it in outside of ES dir.
- Discovery is related to how multiple instances of ES are connected.
- modules/dir contain features

### Architecture

- Node is essentially an instance of ES. Nodes store the data that we add to ES
- Each node belongs to one cluster. Cluster is collection of related nodes that together contains all of our data.
- A cluster for powering search for e-commerce another for APM
- When a node starts up it will either join a new cluster or starts its own
- Each unit of data is stored as document. Document is in json.
- Every document is stored within an index. An index groups documents logically. Documents are grouped together within indices
- Sharding splits indices into smaller pieces. An index is divided into one or more shards, where each shard stores a part of the index' data
- Sharding increases the no. of documents an index can store
- Sharding makes it easier to fit large indices onto nodes.
- Sharding may improve query throughput
- An index defaults to having _one_ shard
- Replication is configured at the index level.
- Replication works by creating copies of shards, referred to as _replica shards_
- A shard that has been replicated, is called a _primary shard_
- A primary shard and its replica shards are referred to as _replication group_
- Replica shards are a complete copy of a shard.
- A replica shard can serve search requests, exactly like its primary shard.
- ES supports taking snapshots as backups
- Snapshots can be used to restore to a given point in time.
- Replication is used to ensure high availability for indices. A side benefit is increased query throughput
- Replication works by copying a given shard's data
- A replica shard is **never** stored on the same node as its primary shard.
- How to add nodes to cluster:
  - Download & extract another ES and start it up. Just change the configurations (such as changing the `node.name`). This approach is recommended as each node will have its dedicated config files and data folder.
  - `./elasticsearch -Enode.name=node-2 -Epath.data=./node-2/data -Epath.logs=./node-2/logs` : this is a shortcut way.
- Each node has roles. Some of which are
  - **Master-eligible** : The node may be elected as the cluster's master node. It is responsible for creating and deleting indices, among others. `node.master: true | false`
  - **Data** : Enables a node to store data. Storing data includes performing queries related to that data, such as search queries `node.data : true | false`
  - **Ingest** : enables a node to run ingest pipelines. `node.ingest: true | false`
    - Ingest pipelines are a series of steps (processors) that are performed when indexing - documents.
    - Processors may manipulate documents, e.g. resolving an IP to lat/lon
- `node.ml` identifies a node as a ML node. This lets the node run ML jobs
- `xpack.ml.enabled` enables or disables the machine learning API for the node.
- **Coordination** : Coordination refers to the distribution of queries and the aggregation of results.
- **Voting-only** : a node with this role, will participate in the voting for a master but it can't be elected as master

### API calls

- `GET /_cluster/health`
- `GET /_cat/nodes?v`
- `GET /_cat/indices?v`
- `GET /_cat/shards?v`
- Creating and removing indices
  - `action.auto_create_index` : if enabled, indices will automatically be created when adding documents. The default is true
- `GET /products/_doc/100` retrieve a doc with id 100
- Performing upsert : if the doc is present its in_stock value is updated or a new doc is inserted
- How does ES know where to store documents? How are documents found once they have been indexed? The answer is routing
- Routing is process of resolving a shard for a document
- `shard_num = hash(routing) % num_primary_shards`
- Primary terms : A way to distinguish b/w old and new primary shards.
- ES versions documents
- Things to be aware of when using bulk api :
  - `Content-Type : application/x-ndjson`
  - Each line **must** end with a new line character. This **includes** the last line
  - A failed action will not affect other actions
- When a text value is indexed a so called analyzer is used to process the text. In other words the value is analyzed.
- An analyser consists of three building blocks : character filter, Tokenizer and Token filters.
- Mapping defines the structure of documents (e.g fields and their data types)
- `object` data type
- `nested` : similar to `object` but maintains object relationships
- `keyword` data type : used for exact matching of values
- Some of the important mapping parameters
  - `format` parameter : used to customize the format for `date` fields
  - `properties` : defines nested fiedls for `object` and `nested` fields
  - `coerce` : used to enable or disable coercion of values, use `coerce: false`, or `index.mapping.coerce : false` to disable it at index level
  - `doc_values` : "Doc values" is another data structure used by Apache Lucene. Used for sorting, aggregations, and scripting. set the `doc_values` parameter to `false` to save disk space. Can't be changed w/o reindexing documents into new index
  - `norms` : normalization factors that are used for relevance scoring. Norms can be disabled to save disk space.
  - `index` : disables indexing for a field. values are still stored within `_source`. Useful if you won't use a field for search queries.
  - `null_value` : `NULL` values cannot be indexed or searched. Use this param to replace `NULL` values with another value. Only works for explicit NULL values.
  - `copy_to` : used to copy multiple field values into a "group field". Simply specify the name of the target field as the value
- Generally, ES field mappings **cannot be changed**
- ECS (Elastic Common Schema) : a specification of common fields and how they should be mapped.
- If we don't provide explicit mapping it is generated dynamically
- When dynamic is set to false, new fields are not indexed but still part of `_source`. no Inverted index is created for the last_name field
- If dynamic setting is set to strict, then we can't added documents unless we provide mapping for all fields
- set `numeric_detect` to true to make ES look for numbers in strings
- `standard` analyzer : splits text at word boundaries and removes punctuation. Done by the `standard` tokenizer.
- Lowercases letter with `lowercase` token filter
- Contains the `stop` token filter
- `simple` analyzer
- `whitespace` analyzer
- `keyword` analyzer
- `pattern` analyzer. This provides a lot of flexibility
- Term level queries are not analyzed and thus case sensitive
- Full text queries are analyzed and is thus case insensitive
