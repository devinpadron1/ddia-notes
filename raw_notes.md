## Vocabulary
**Application** = runnable software that performs a function
**System** = a group of components (apps, dbs, libraries, etc.) that interact to perform a function
**Data** = recorded information
**Data model** = defines how data is structured, related, and manipulated within a system
**Query language** = language to interact with data through models

## Chapter 1: Reliable Scalable and Maintainable Applications 
The chapter revolves around defining 3 key terms: **reliability**, **scalability** and **maintainability**.

### Reliability
- Is the application behaving as intended?
- Is the application resilient?

#### Faults and Failures
- faults are local but don't break the app
- failure brings the system to a halt
- The author points out an exercise in deliberately introducing faults in the system, like killing random processes.
  - ex: Netflix Chaos Monkey

Types of faults:
1. Hardware 
- ex: faulty storage disk, power loss
- generally handled by increasing redundancy (add more hardware)

2. Software 
- ex: leap second on June 30, 2012 crash due to bug in Linux kernel
- generally, the damage is more widespread

3. Human 
- humans are unreliable, design the api to minimize blast radius, have a staging environment, etc.

### Scalability
- Can the application handle increased volume of data?
- Load, needs to be defined / measured
  - load describes how the system is used
  - load is how we measure how a system is used
  - ex: requests per second, reads vs writes to a db, simultaneous active users, cache hit rate
- Performance
  - describes how responsive a system is
  - extremely important, Amazon understands this concept very well (ex. single click to buy). They lose a lot of money for every second they're down.
- architectures change depending on scale, its not one-size-fits-all. 
  - needs to be rethought of as users experience order of magnitude changes
- an elastic system is one that can dynamically allocate resources based on load

### Maintainability
- A measure of how quickly an engineer can get up-to-speed and contribute to the codebase.
- How quickly can one diagnose / patch issues 

### Things to expand upon
data-intensive vs compute-intensive
database vs message queue

### Summary in my own words
This chapter discussed 3 core concepts: reliablility, scaleability and maintainability.
A system is reliable when it functions as intended. A system is robust when if can handle local faults while still functioning.
- Example: An artifact service goes down but BGPT is still able to function
A system is scaleable when it is able to handle increased load.
A system is maintainable when it is easy to operate, contribute, diagnose.

What does a compute-intensive application look like? What does a data-intensive application look like?
- For something compute intensive I think of training a large language model. For SOTA models this takes months of heavy compute. If we had the perfect GPU with infinite computational capacity then training a large language model would be much faster. Compute is the current constraint of training.
- For something data-intensive I think of a social media application. Something that handles a lot of state and a lot of data handling. Not a lot of computation is ocurring in the sense of CPU usage(?). If we had the perfect DB, then apps would load and update state instantly(?).

## Chapter 2: Data Models and Query Languages
Software leads to applications and applications need to store and retrieve data.

The 3 most common data models are:
1. Relational
2. Non-relational (document)
3. Graphs

### Relational (SQL)
- good for many to many relationships 

### Non-relational (NoSQL)
- good for one to many relationships (tree structure)
- are generally closest to the data structure they're trying to represent in an application
- read in one go, not ideal for small reads since you can't partially read a table

### Declarative vs Imperative
#### Declarative languages
- declarative describes the final state
- declarative languages have the advantage of abstracting away how it reaches a final state. for instance an sql query can hide optimizations performed under the hood. 
- ex. SQL, CSS, etc. 

#### Imperative languages
- imperative describes the steps it takes to get to the final state
- user has more explicit control but it generally requires more code to get something done vs the alternative 
- ex. C, Python, etc.

### Graphs
- best used when traversal depth for a query is variable 

### Summary / thoughts
- Why did BGPT go with SQL? What would happen if we migrated to a NoSQL db.
  - many to many relationships. sessions have messages, messages have models. agents have mcps, mcps have tools
- How do I demonstrate the benefits / downsides of both approaches at the extremes?

## Chapter 3: Storage and Retrieval
Two schools of thought with transactional DBs
Logging: append (newer school of thought)
B-Trees: replace

Transactional vs Analytic use cases of DBs (read-writes)
- transactional (OLTP)
  - user facing
  - typically frequent but targeted queries 
  - the bottleneck: disk seek 
- analytics (OLAP) 
  - internal, typically used by data / business analysts
  - query data in bulk
  - the bottleneck: disk bandwidth 

Data Warehousing
- DBs obtimized for analyst usage patterns (fast bulk reads)

### thoughts
It'd be good to perform an exercise to really hone in on the different dbs. The goal is to understand their characteristics, showcase their strengths and showcase their weaknesses.

Author mentioned that logging dbs leverage the performance of disk. How quick appending is. 

## Chapter 4: Encoding and Evolution
- encoding is not the same as encrypting
  (?) whats the difference?
  - their use case, encrypting is meant to hide data, encoding is the process of changing how its represented
- programs work with data in 2 ways:
  - memory stored in data structures
  - bytes sent over the network or saved to a file
    (?) why do these two differ? why can't we store the bytes from memory in a data structure to a file? What does that look like? Its all ultimately a sequence of bytes. When you
      - for complex objects - it gets complicated (think about how you would store a struct with a pointer to another object)

- HTTP is the protocol for communicating over the internet
- REST, RPC, Message queues are different ideas around how to go about this 
- importance of keeping things backward and forward compatible