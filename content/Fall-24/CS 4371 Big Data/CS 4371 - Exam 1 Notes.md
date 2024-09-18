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

## TA Lecture - Sept 16

- given a word count situation with three documents. the naive solution would have n key value pairs. where n is the number of words within all the documents
  - This would be a massive amount of network traffic to transmit every \<word, 1> Key-value pairs
  - Solution 1
    - Each mapper will reduce its own word count, to remove duplicate keys within the single document / mapper
      - Ex. \<a, 1> <a, 1> <a, 1> ---> \<a, 3>
      - If a mapper handles multiple documents, there will still be duplicates between documents
    - Each mapper then will have the total sum of each letter or word it saw within its domain/ document. Not the global sum however. for that we need to use the reducers
    - So these intermediate key value pairs (Ex. \<a, 3> \<a, 5> etc.) are sent to the reducer sorted by key, which can combine the kv pairs to the lowest form (Ex. \<a, 8>)
  - Solution 2
    - using a mapper class that is initialized once, and has a map method that can iterate over several documents we are able to emit a single set of key value pairs to cover all documents in the domain. Thus, reducing network traffic to the reducers.
- Computing the Average count of a word within the documents
  - Method 1
    - For each document, find the number of "apples" / # key-value pairs. (this number of pairs can have duplicates. for example a mapper with 2 documents could have 2 <k,v> pairs with the same key)
    - This is a problem because the average is different based on the number of files, even with constant number of words.
  - Method 2
    - for each document, use a mapper to emit <k, 1>
    - Using a combiner emit the sum of all key value pairs. emitting <k, v>
    - Using a reducer to take results from all mapper/combiners into one list of the global sums of each key in the domain
  - Method 3
    - Initialize a Term dictionary and a count dictionary
    - the map function will take in a string and an integer representing the count of that key within the document.
      - The count dictionary value is incremented, as will the string dictionary
  - Stripes Method
    - Each mapper counts and sums a key value pair for every word / letter
    - The reducer then does a term wise summation (summation of all a, b, c etc.)
- Map Reduce in Java
  - Setup
    -
  - Map / Reduce
  - Cleanup
