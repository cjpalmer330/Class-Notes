# Introduction

## <span style="color:#7dafff">Big Data 4 Vs</span>

### 1 - Volume

### 2 - Variety

- Various data forms, types, and structures
- can have static or streaming data
- must support a single application generating and collecting many types of data

### 3 - Velocity

- Late decisions -> missing opportunities
- for websites like twitter, where gigabytes are being created every minute, you must be able to queue, and process data extremely fast
- Kafka is a common buffering system

### 4 - Veracity

- Data in doubt
- can all the data be trusted

## Big Picture

- What's driving Big Data
  - Small value, low complexity tasks like Business Intelligence
    - data mining techniques
    - structures data, typical sources
    - ad-hoc querying and reporting
  - high value, high complexity tasks like data mining
    - more real time tasks
    - very large datasets
    - all types of data, from many sources
    - complex statistical analysis
- What will we learn
  - Hadoop/MapReduce technology, NoSQL, Big Table, Cassandra, SPARK, Stream
  - Learn the platform
  - Learn writing Hadoop jobs/SPARK in different languages
  - Learn advanced analytics tools on top of Hadoop
- Parallel
  - multiple CPUs each sharing some memory space
  - Problems with Parallelization
    - How do we assign work to CPUS?
    - What if workers need to share partial results?
    - How do we know when all workers are done? What if a worker dies during the process?
- Distributed
  - Multiple CPUs each with their own memory space
- Inverted Index
  - Google when crawling the web will create a word count for certain terms of every page that it has processed.
  - It then has an Inverted Index for every term they want to track, perhaps 100s of thousands of terms. These terms are sorted by some metric, and these terms have a pointer to different pages that contain those keywords and what page they appear on.

## Map Reduce

- Map Reduce comes from functional programming
- Two important functional programming functions
  - Map: do something to everything in a list
    - function is applied to every element in a list
    - result is a new list
    - Works in parallel for each element in the list, where they all are put through some function concurrently
  - Fold: combine the results of a list in some way
    - Steps of operation
      1.  accumulator set to some value
      2.  function applied to list element and the accumulator
      3.  result stored in the accumulator
      4.  repeated for every item
      5.  results is the final value in accumulator
    - Ex: If we have the list (1,2,3,4,5)
      - Acc = 0
      - Acc + 1 = 1
      - Acc + 2 + 3
      - Acc + 3 = 6
      - Acc + 4 = 10
      - Acc + 5 = 15
      - Result = 15
    - Doesn't work well with parallelization because each sub value must be passed on to the next operation
