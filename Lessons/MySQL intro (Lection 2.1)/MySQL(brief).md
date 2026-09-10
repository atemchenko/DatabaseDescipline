### MySQL

#### Intro

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; MySQL is a free, open-source database management system that is easy to install, implement, and maintain. In addition, MySQL is fast, extremely reliable, and widely deployed by many companies and organizations worldwide.
It is not an exaggeration to say that MySQL has brought the power of a fully featured, scalable relational database management system to the reach of anyone with a computer and the desire to build a data-driven application or website. MySQL was originally developed by MySQL AB, a company founded in Sweden in 1995, and remained independent until it was acquired by Sun Microsystems in 2008. Oracle purchased Sun Microsystems in 2008 and found itself owning MySQL. Oracle provides a free community and a subscription-based enterprise edition of MySQL. Though the core elements of the two editions are identical, the enterprise edition includes additional scaling, performance, security, backup features, and 24x7 support. To address the broadest possible audience, this book is based on MySQL 9 Community Edition.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The name MySQL comes from a combination of “My” and “SQL.” Where “My” is named after My, the daughter of Monty Widenius, one of the original developers of MySQL. SQL stands for Structured Query Language; the language used for managing databases.

#### MySQL architecture

MySQL utilizes a multi-layered, client-server architecture designed to separate core logic, connection management, query execution, and physical data storage. The most defining feature of this system is its pluggable storage engine architecture, which allows developers to swap out underlying database storage engines based on application requirements without rewriting SQL queries.

![alt text](images/MySQL_architecture_flow.JPG)

##### Application / Client Layer

This top layer interfaces directly with users and client applications.

###### Connection pool

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Every MySQL query begins its journey at the application layer, where your business logic interfaces with the database. This layer is responsible for establishing connections, managing connection pools, and sending SQL statements to the MySQL server.

_**Connection Pool Benefits:**_

- _Reduced Latency:_ Eliminates connection establishment overhead
- _Resource Efficiency:_ Limits concurrent connections to prevent server overload
- _Scalability:_ Allows applications to handle more concurrent users
- _Stability:_ Provides connection validation and automatic retry mechanisms

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_**Typical performance improvement when using connection pools vs. creating new connections per request:**_

- Connection Establishment Time 100-300ms (Time to establish a new MySQL connection over network)

- Pool Connection Time 1-5ms (Time to retrieve connection from an established pool)

###### Database Drivers and Protocol Handling

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Database drivers implement the MySQL protocol, handling the low-level communication between your application and the MySQL server. Modern drivers support features like prepared statements, SSL encryption, and load balancing across multiple database servers.

_**Key Driver Features:**_

- Prepared Statements: Improve performance and prevent SQL injection
- SSL(Secure Sockets Layer)/TLS(Transport Layer Security) Support: Encrypt data in transit
- Automatic Failover: Handle server failures gracefully
- Load Balancing: Distribute queries across multiple servers
- Connection Validation: Ensure connections remain healthy

##### MySQL Server Layer (The Brain / SQL Layer)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The MySQL server layer is where the magic happens. This is where your SQL queries are parsed, analyzed, optimized, and transformed into execution plans. This layer is storage engine agnostic, meaning the same query processing logic works regardless of whether you're using InnoDB, MyISAM, or any other storage engine.

**Query Processing Flow**

![alt text](images/Query_processing_flow.JPG)

_SQL travels left-to-right through the server layer, then drops into the storage engine. A query-cache hit (dashed green) bypasses parse + optimize entirely — MySQL 8.0 dropped this path._

###### Connection Handler: Managing Client Sessions

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The connection handler manages individual client sessions, handling authentication, authorization, and session state management. Each connection maintains its own thread (in traditional threaded model) or is handled by a thread pool worker.

Connection Handler Responsibilities:

- User authentication and authorization
- Session variable management
- Character set and collation handling
- Transaction state tracking
- Resource limit enforcement

###### Query Parser: Breaking Down SQL Statements

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The parser is MySQL's first line of defense against malformed SQL. It performs lexical analysis, syntax checking, and builds an Abstract Syntax Tree (AST) that represents the logical structure of your query.

###### Query Optimizer: The Performance Wizard

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The query optimizer is perhaps the most sophisticated component in MySQL. It takes the parsed query and generates the most efficient execution plan possible, considering available indexes, table statistics, join ordering, and various access methods.

###### Query Cache (Legacy Feature)

**!!! Important Note:** Query cache was deprecated in MySQL 5.7 and completely removed in MySQL 8.0 due to performance issues in multi-core environments. However, understanding its concept helps grasp modern caching strategies.
The query cache stored exact query text along with result sets. If an identical query arrived, MySQL could return cached results immediately. Modern applications use external caching layers like Redis or Memcached for similar functionality.

##### Storage Engine Layer (InnoDB)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The storage engine layer is where logical database operations become physical data manipulation. InnoDB, MySQL's default storage engine since version 5.5, provides ACID transactions, row-level locking, crash recovery, and multi-version concurrency control (MVCC).

###### InnoDB Buffer Pool: The Performance Game Changer

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The buffer pool is InnoDB's crown jewel for performance optimization. It's a sophisticated memory cache that stores frequently accessed data pages, index pages, and internal data structures, dramatically reducing disk I/O operations.

**Buffer Pool Management Algorithms**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;InnoDB uses a sophisticated LRU (Least Recently Used) algorithm with a midpoint insertion strategy to manage buffer pool pages efficiently:

New Pages(_Recently read pages enter at midpoint_) -> Young pages (_Frequently accessed pages move to head_) -> Old Pages(_Less frequently used pages move to tail_)

###### Transaction Management and MVCC

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;InnoDB's transaction system ensures ACID properties through sophisticated mechanisms including undo logs, redo logs, and multi-version concurrency control (MVCC).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MVCC allows multiple transactions to access the same data simultaneously without blocking each other. Each row contains hidden system columns that track transaction IDs and rollback pointers.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;InnoDB stores data in a clustered index structure where table data is organized by primary key.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;InnoDB implements a sophisticated locking system that balances data consistency with performance.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;InnoDB automatically detects deadlocks and resolves them by rolling back the transaction with the smallest number of modified rows.

##### File System Layer - Physical Storage Management

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The file system layer bridges the gap between logical database operations and physical storage. It manages various file types, each serving specific purposes in maintaining data integrity, performance, and recoverability.

###### Data Files (.ibd) - Table and Index Storage

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When using the innodb_file_per_table setting (default since MySQL 5.6), each InnoDB table gets its own .ibd file containing both table data and indexes. This provides better management, backup flexibility, and crash recovery isolation.

###### Redo Logs - Crash Recovery and Durability

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Redo logs are critical for ensuring durability and crash recovery. They record all changes to data pages before the changes are written to the actual data files, following the Write-Ahead Logging (WAL) protocol.

###### Undo Logs - Transaction Rollback and MVCC

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Undo logs serve two critical purposes: enabling transaction rollback and supporting MVCC for consistent reads. They store the previous versions of modified rows, allowing other transactions to see consistent snapshots of data.

Undo Log Functions:

- Transaction Rollback: Restore original values on ROLLBACK
- MVCC Support: Provide old versions for consistent reads
- Crash Recovery: Roll back uncommitted transactions after restart
- Long-Running Queries: Maintain consistent view for extended reads

###### Binary Logs - Replication and Point-in-Time Recovery

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Binary logs record all changes to the database in a sequential, append-only format. They're essential for MySQL replication and point-in-time recovery scenarios.

##### Hardware Layer - The Physical Foundation

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;At the bottom of our architectural stack lies the hardware layer, where all database operations ultimately execute. Understanding hardware characteristics and limitations is crucial for optimal MySQL performance.

_**CPU Architecture and Database Performance**_

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modern CPUs with multiple cores and sophisticated cache hierarchies significantly impact database performance. MySQL's threading model and InnoDB's design take advantage of multi-core systems effectively.

| Memory type | Access time | Description                                                     |
| :---------- | ----------- | --------------------------------------------------------------- |
| L1 Cache    | ~ 1 ns      | Fastest memory access for frequently used instructions and data |
| L2 Cache    | ~ 4 ns      | Shared cache between CPU cores for recently accessed data       |
| RAM         | ~ 100 ns    | Main memory access for database buffer pools and caches         |

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Memory hierarchy directly impacts MySQL performance through various caches and buffers. Understanding memory access patterns helps optimize configuration and query design.

_**Memory Hierarchy Access Times**_

| Memory type   | Access time |
| :------------ | ----------- |
| CPU Registers | ~0.3ns      |
| L1 Cache      | ~1ns        |
| L2 Cache      | ~4ns        |
| L3 Cache      | ~12ns       |
| RAM           | ~100ns      |
| SSD           | ~100,000ns  |
| HDD           | ~100,000ns  |

_**Storage Technology Impact**_

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Storage technology has the most dramatic impact on database performance, with differences of several orders of magnitude between HDD, SSD, and NVMe storage.

| Storage Type         | Random IOPS   | Sequential Throughput | Latency Use Case |
| :------------------- | ------------- | --------------------- | ---------------- | ------------------------------- |
| **HDD (7200 RPM)**   | ~200 IOPS     | ~150 MB/s             | ~10ms            | Archive, large sequential scans |
| **SATA SSD**         | ~10,000 IOPS  | ~550 MB/s             | ~0.1ms           | General purpose databases       |
| **NVMe SSD**         | ~100,000 IOPS | ~3,500 MB/s           | ~0.02ms          | High-performance OLTP           |
| **Optane/3D XPoint** | ~550,000 IOPS | ~6,500 MB/s           | ~0.01ms          | Ultra-low latency applications  |

_**IOPS**(pronounced "eye-ops") stands for Input/Output Operations Per Second_\
**SATA** (Serial Advanced Technology Attachment)\
**NVMe** (Non-Volatile Memory Express)

##### Complete Query Execution Flow

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Now that we've explored each layer in detail, let's trace a complete query through the entire MySQL architecture stack, from application to hardware and back.

Query Example:

```sql
SELECT c.name, c.email, o.order_date, o.total_amount
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE c.status = 'active'
AND o.order_date >= '2024-08-01'
ORDER BY o.order_date DESC
LIMIT 20;
```

_**Step-by-Step Execution Trace:**_

_1. Application Layer (1-5ms)_

- Connection retrieved from pool (1ms)
- Query sent via MySQL protocol (1-2ms)
- Driver handles communication (1-2ms)

_2. MySQL Server Layer (5-50ms)_

- Connection handler receives query (1ms)
- Parser validates syntax and builds AST (2-5ms)
- Optimizer analyzes execution plan:
  - Checks available indexes on customers.status and orders.order_date
  - Estimates join cost: customers -> orders vs orders -> customers
  - Decides on index usage and join algorithm
  - Generates optimal execution plan (5-20ms)
- Execution plan sent to storage engine (1ms)

_3. Storage Engine Layer (10-200ms)_

- Buffer pool checked for cached pages
  - customers table pages (hit ratio ~95%)
  - orders table pages (hit ratio ~90%)
  - Index pages for status and order_date (hit ratio ~98%)
- Row-level locks acquired for REPEATABLE READ isolation
- MVCC (Multi-Version Concurrency Control) provides consistent snapshot of data
- Join algorithm executed (nested loop or hash join)
- Results sorted and limited to 20 rows

_4. File System Layer (varies by storage)_

- Cache misses trigger physical reads from .ibd files
- HDD: 10-100ms per physical read operation
- SSD: 0.1-1ms per physical read operation
- NVMe: 0.02-0.1ms per physical read operation

_5. Hardware Layer (hardware dependent)_

- CPU executes join algorithms and sorting
- RAM provides working space for operations
- Storage device serves requested data pages
- Network interface returns results to application
