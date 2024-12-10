---
tags: spark
---
# Spark basics

**Spark**

- open source distributed computing engine - process and analyze large amounts of data
- spark → in memory model :::: where as hadoop → in disc processing model
- spark - 100 times faster in memory processing and 10 times faster in disc processing - than hadoop and trad systems
- why is spark speed? - in-memory and parallel processing
- well-designed layered architecture
- master/slave concept - driver and worker concept

**Spark Architecture**

Spark follows a well-designed layered architecture that enables efficient and scalable distributed data processing. It consists of three main layers: the driver, the cluster manager, and the worker nodes.

1. **Driver Layer**: The driver is responsible for coordinating and managing the Spark application. It interacts with the user and submits tasks to the cluster. The driver also maintains the overall execution plan and orchestrates the flow of data and computation.
2. **Cluster Manager**: The cluster manager is responsible for acquiring and managing resources in the cluster. It allocates resources to the Spark application, such as CPU and memory, and monitors their usage. Popular cluster managers for Spark include Apache Mesos, Hadoop YARN, and standalone mode.
3. **Worker Layer**: The worker layer consists of multiple worker nodes that execute tasks assigned by the driver. Each worker node runs tasks in parallel and manages the local resources allocated to it. The worker nodes communicate with the driver and perform data processing operations.

**Spark Architecture Diagram**

```
         +-----------------+
         |                 |
         |     Driver      |
         |                 |
         +-----------------+
                   |
                   |
            +--------------+
            |              |
            | Cluster      |
            | Manager      |
            |              |
            +--------------+
                   |
                   |
          +-----------------+
          |                 |
          |   Worker        |
          |   Nodes         |
          |                 |
          +-----------------+

```

The driver, cluster manager, and worker nodes are designed to be loosely coupled, allowing Spark to scale horizontally and handle large-scale data processing. This architecture, combined with Spark's in-memory processing and parallel computing capabilities, contributes to its high performance and speed compared to traditional systems like Hadoop.

Spark's layered architecture and distributed computing model make it a powerful tool for processing and analyzing large amounts of data efficiently.

**Life cycle of Spark application**

- user submits application
- driver initiates spark session
- DAG creates logical plan
- task executor requests for resources from cluster manager
- cluster manager allocates resources to execute task
- driver establishes connection with worker and assigns task
- workers executes the task and returns results to driver
- driver returns the result to user
- application comes to end

**Spark attributes**

- scalable - worker nodes can increase as per the requirement until maximum limit
- fault tolerant - lazy evaluation - if one worker node goes down - data can be recovered using lineage graph in DAG
- polyglot - supports multiple programming languages
- real-time - can handle both streaming and batch processing
- speed -  distributed parallel framework and in-memory
- rich libraries - SQL, ML, etc

**Terminologies:**

- driver and worker process
    - JVM process.
    - in worker node - each executor runs its own JVM process
- application
    - piece of code for execution
- jobs
    - when application submitted to spark, driver process converts the code into job
- stage
    - jobs are divided into stages
    - number of stages are determined by number of shuffling operations
    - JOIN is an example of shuffling operation
- task
    - stages are further divided into tasks
    - in stage — all tasks execute same operation
    - each task will process 1 partition at a time
- transformation
    - transforms input RDD and creates a new RDD
    - until action is called, transformations are evaluated lazily
- DAG
    - keeps track of all transformation
    - for each transformation - logical plan is created and lineage graph is maintained by DAG
- action
    - when data output is needed - action is called
    - action is executed based on DAG and processes the actual data
- RDD
    - Resilient Distributed Dataset - basic data structure of spark
    - distributed across nodes in the form of partition
- executor
    - each worker node can contain many executors
    - each executor can have one or more cores
    - can be configured by spark settings
- partition
    - RDD/dataframe is stored in-memory of cluster in form of partition
- core
    - each core (if single thread) can process one task at a time, can pick one partition at a time
- on-heap memory
    - executor memory that lies within JVM process
    - read or write within this memory - de-serialize or serialize the data
    - garbage collector scans data and removes obsolete data - unnecessary operation
- off-heap memory
    - lies outside JVM process
    - handled by OS directly

**Spark libraries**

- SQL
- Streaming
- MLLib
- GraphX

**Languages**

- Scala
- Java
- Python
- SQL
- R

- in one application/notebook - multiple languages can be mixed - using magic commands

**Driver node architecture**

![Untitled](/images/spark-driver-architecture.png)

**Worker node architecture**

![Untitled](/images/spark-worker-architecture.png)

**On-heap memory**

- controlled by JVM process
- reserved memory - spark memory - cannot use this - used for failure recovery
- user memory - 40%
    - storing user objects. metadata
    - data structures created and managed by user’s code
- unified memory - 60%
    - storage memory - JVM headspace reserved for cached data
    - execution memory - JVM heap space used by data structures during shuffling operations
- memory is highly configurable in spark settings

******************Off heap memory******************

- controlled by OS
- each executor within worker node has access to common off heap memory
- used by spark explicitly for storing its data
- spark.memory.offHeap.use = True
- spark.memory.offHeap.size
- accessing off heap is slightly slower than on heap but still faster than reading/writing from disk
- garbage collector scan can be avoided by using off heap memory

**RDD vs Dataframe vs Dataset**

- APIs provided by spark for data processing and analytics
- all are same and provides same output
- differ in a way of handling and processing data
- differ in terms of performance, user convenience and language support
- RDD - introduced in spark 1.0
- dataframe - introduced in spark 1.3
- dataset - introduced in spark 1.6
- dataset = best of RDD + best of dataframe

Similarities in RDD vs dataframe vs dataset

- fault tolerant
- distributed in nature
- in-memory parallel processing
- immutable
- lazy evaluation
- internally processing as RDDs for all APIs

![Untitled](/images/rddvsdfvsds.png)

**Spark - Transformation and action**

Transformation:

- after transformation - new dataframe is created after executed
- lazy evaluation based on DAG
- union, filter, etc..

Action:

- when want to work with actual data - call action
- action returns data to driver for display or storage purpose
- count, collect, save, show etc..

**********Types of transformation:**********

- Narrow transformation
    - no need to shuffle data across nodes
    - in-expensive
    - example: filter operation
        - each executor can work independently with its own partition
- Wide transformation
    - need to shuffle data across nodes
    - expensive and effects performance
    - example: group by
        - each executor depends on partitions of other executors
        - need to shuffle data from multiple partitions

![Untitled](/images/spark-transformationm-list.png)
