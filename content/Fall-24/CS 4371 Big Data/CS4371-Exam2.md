# Spark

> [!important]- What are some problems with Hadoop?
>
> - You cannot use loops, every iteration needs its own mapper, lots of memory use time
> - Given two queries, Hadoop will access the whole table twice. once for each query which can be very time consuming

> [!important] Spark's Goal
>
> - To create a Fault Tolerant, and efficient method for processing data
> - Do so in the main memory using RDD

- We want to perform the map reduce in main memory whenever possible, to avoid pointless waiting for disk fetch.
  - Query results are saved into the main memory so that if we have another query who's results are entirely inside the previous result, we can skip disk check
  - This goes against fault-tolerance as its possible the previous result doesn't contain the new query so we must go to the disk anyways
- Allows for iterative algorithms
- Integrates with Scala programming language
- Because

## Spark Fault Tolerance

- Uses the same multi-machine redundancy that we theorized in the Hadoop case, however data is placed in the main memory, not the disk.
- Lineage Graph
  - The connection from the original partition in the HDFS with all intermediate results along the way

### Resilient Distributed Datasets

- Can be created from raw files or other RDD
- Very forgiving towards parallel processing of data sets using an image of the system, using cached values and the dataset in main memory
- Restricted form of distributed shared memory
  - immutable, partitioned collections of records
  - through coarse transformations(map, filter, join)
- Efficient recovery using lineage graph
  - Log one operation to apply
- Transformations
  - Create a new dataset using the old one
  - EX: map, reduce, Collect
  - Flatmap can create multiple elements from a single input element depending on the function
    - One-to-many map
- Actions
  - mapValues
    - will enact a lambda functinothat only works on the values. EX: x = x + 1
  - flatMapValues()
  - ReduceByKey()
    - does the reducing that we needed a separate class for in hadoop

# Page Rank

- Ingoing vs outgoing
  - most pages have links that go to other websites. if they have links like social media sites or other external websites these are called outbound
  - other websites linking to my webpage are called inbound links
  - Every page can be described in part by its inbound and outbound link degrees
- Using both ingoing and outgoing link count we can find the page's **_Rank_**
- Finding Page Rank
  1.  Each page starts with a value of 1.
  2.  Each page outputs a value of 1/outgoing degree to each outbound link in the diagram
  3.  These inbound links are then your new score to determine your rank

# Spark SQL Concepts

- Create a dataframe to hold the information
  - Dataframes can be created with SparkSession
- Dataframe being implemented with an RDD of Rows

# Messaging System

- A messaging system handles messages between hosts
- Point to Point
  - This system uses a queue to persist messages
  - one or more consumers can consume the messages in the queue
- Publish-Subscribe
  - This system messages are persisted in a topic
  - consumers can subscribe to one or more topics and consume all the messages
  - message producers are called publishers and message consumers are called subscribers

## Kafka

> [!important]- Kafka Rules & Terminology
>
> - Topics are identified by name and kept within patitions
> - The order of messages is maintained at the partition level
> - Partitions are immutable.
> - Messages are stored within the partitions with key, value, and timestamp
> - Each partition has a leader topic, the rest are followers
> - Broker- a single machine / worker

- Uses publish-subscribe architecture
- is an enterprise messaging system that provides high throughput with partitions and fault tolerance with replication.
- deals with real time streaming data. Works with Spark Streaming
- Messages belong to a particular category called Topics
- Partition
  - Each topic is put into a partition
  - For each topic, Kafka keeps >=1 copy of each topic
  - each partition has a unique sequence ID called the offset
- Producer / Consumer
  - Each consumer group has its own pointer to check where in the partition it wants to read
  - producer always inserts at the end of the queue, taking up a single pointer
- Zoo Keeper
  - an open source coordination service for managing and coordination Kafka brokers
  - notifies producers and consumers about new brokers, when there is a failure, etc

# NoSQL Databases

Study Topics

1. Are Partitions mutable or immutable?
