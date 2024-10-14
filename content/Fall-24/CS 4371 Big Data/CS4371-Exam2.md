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
