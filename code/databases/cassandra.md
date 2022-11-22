# Cassandra

## Terms & Definitions

### Infrastructure terminology

| Term        | Definition                        | AKA                                                        |
| ----------- | --------------------------------- | ---------------------------------------------------------- |
| Node        | Logical atomic unit of processing | Database server instance                                   |
| Rack        | A set of Nodes                    | Load-balanced database server instances                    |
| Data Center | A set of Racks                    | Highly available load-balanced database server instances   |
| Cluster     | A set of Data Centers             | Globally available load-balanced database server instances |

### Database terminology

| Term           | Definition                                                                                              | AKA                       |
| -------------- | ------------------------------------------------------------------------------------------------------- | ------------------------- |
| Keyspace       | Contains data related to a single application                                                           | Schema                    |
| Table          | Contains data related to a single object (originally known as Column Family)                            | Database table/Collection |
| Row            | Contains data for a single object instance with a structure equal to the Column Family the Row is under | Row (duh?)                |
| Primary Key    | Composed of the Partition Key and Clustering Keys                                                       | Nothing like this         |
| Partition Key  | Non-unique identifier for a Row                                                                         | Primary key               |
| Clustering Key | Identifier for Rows with                                                                                | A `GROUP BY`-ish key      |

### Opertaional terminology

| Term                | Definition                                                                    | AKA                               |
| ------------------- | ----------------------------------------------------------------------------- | --------------------------------- |
| Bloom Filter        | Reduces the seek-time for read operations                                     | -                                 |
| CQL                 | Cassandra Query Language                                                      | (Yet another) domain-specific SQL |
| Gossip              | Protocol used to achieve eventual consistency                                 | The syncing protocol              |
| Partition Key Cache | Stores the location of partitions which are commonly queried. Default feature | -                                 |
| Row Cache           | A cache reserved for frequently queried data rows. Optional feature           | -                                 |
| Seed Node           | A node that bootstraps other nodes                                            | Master database instance          |

## Usage

### Consistency Check

* Before querying, Cassandra checks if replicas consistency is satisfied. If not, an exception is thrown

### Read Path

* Reading is more expensive than writing because of the Consistency Check and Read Repair

### Read Repair

* When reading, data is read from the fastest responding replica (via the [dynamic snitch](https://wiki.joeir.net/cassandra/#snitches)) and hashed into a digest. Digests are requested from all other replicas. If all match, the data is returned to the client. If some don't match, data is requested from all replicas and the data with the latest timestamp is returend to the client. Read Repair requests are then issued to replicas whose digest do not match the digest of the returned data (with the latest timetsamp).

### Tombstone

* Cassandra does not delete data immediately, it tags a Row for deletion and this is called a Tombstone

## Availability Scaling

### Consistency

* Cassandra is eventually consistent

### **One, Two, Three**

* Specified number of replicas must agree

### **Quorum**

* A majority of replicas must agree

### **Local\_ (modifier)**

* `Local` refers to the Data Center
* `Local_two` - two replicas within the Data Center must agree
* `Local_quorum` - majority of replicas within the Data Center must agree

### **Each\_ (modifier)**

* `Each` refers to the Data Center
* `Each_two` - all Data Centers must have two replicas within that Data Center agree
* `Each_quorum` - all Data Centers must have a majority of replicas within the Data Center agree

### Data Partition

* Made up of rows that share a common Partition Key

### Data Replication

* Configured per keyspace (replication factor per data center)

### Data Centers

* Simulates real-world data centers
* Holds multiple racks
* Enable segregation of workloads that require the same data set
* Enable lower latency by processing data as close to the source region as possible
* Enables a disaster-recovery-ready setup

### Node

* Owns set(s) of Data Partitions
* Responsible for multiple token-ranges via Vnodes
* Each node by default is assigned 256 tokens (`num_tokens` option)

### Partition

* Ideally less than 10 MB
* Definitely less than 100MB
* Size of this is a crucial attribute for Cassandra performance

### Partition Key

* Each Table contains one
* Can be standalone or composite
* Converted to a Token by a partitioner
* Default partitioner is `Murmur3Partitioner`

### Replication

* Defined by the replication factor
* One primary replica which exists in the Node that owns the associated Token

### **Replication Factor**

* Ideally an odd number
* Number of Racks should be in multiples of the Replication Factor

### **Replication Strategy**

* Defined at the keyspace level
* Either `SimpleStrategy` or `NetworkTopologyStrategy`
* `SimpleStrategy` does not consider Racks/Data Centers. Places data replicas on nodes sequentially
* `NetworkTopologyStrategy` does the above

### **Replica Placement Strategy**

* Defines placement of a Data Partition on other nodes

### Snitches

* Determines the Data Center and Rack a Node belongs to
* Defined at the Node level
* Defaults to `GossipingPropertyFileSnitch`
* Dynamic snitches exist to keep track of node latencies dynmically

### Token

* Unique identifer referencing a Data Partition
* Range: `-2e63` to `+2e63 - 1` - each node owns a portion of this range and primarily owns data corresponding to the range
* Token range is split as cluster grows (eg. if one node => one node owns entire range, if X nodes => X nodes own the entire range)

### VNode

* A virtual node structure that enables splitting of a node's token-range into smaller ranges
* Each physical node is assigned a number of virtual nodes

## Performance Scaling

### Partition sizes

* Partition size affects JVM heap size
* Partition size affects GC
* Big partition makes garbage collection inefficient (GC works over a larger JVM heap)
* Partition size affects Cassandra Repairs

### Querying

* Use as little Partition Keys as possible - 1 if possible
* Cassandra is more efficient because data is retrieved from a single Partition

## Error messages

### Data center differs from previous data center

```
ERROR [main] 2020-09-24 02:10:24,030 CassandraDaemon.java:759 - Cannot start node if snitch's data center (dc1) differs from previous data center (ap-southeast-1). Please fix the snitch configuration, decommission and rebootstrap this node or use the flag -Dcassandra.ignore_dc=true.
```

* Run `kubectl edit statefulset cassandra` and add the flag

### Snitch's rack differs from previous rack

```
RROR [main] 2020-09-24 02:13:42,845 CassandraDaemon.java:759 - Cannot start node if snitch's rack (rack1) differs from previous rack (ap-southeast-1a). Please fix the snitch configuration, decommission and rebootstrap this node or use the flag -Dcassandra.ignore_rack=true.
```

## References

1. [Apache Cassandra (open-source version)](https://cassandra.apache.org/)
2. [Datastax (commercial Cassandra)](https://www.datastax.com/)
3. [Cassandra Data Partitioning](https://www.instaclustr.com/cassandra-data-partitioning/)
