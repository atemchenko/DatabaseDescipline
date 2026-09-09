# Lection 1("Database theory")

## 1. Introduction

#### Database theory

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_**Database theory**_ encapsulates a broad range of topics related to the study and research of the theoretical realm of databases and database management systems.

#### Data and information

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _**Digital data or digital information**_, in information theory and information systems, is data or information represented as a string of discrete symbols, each of which can take on one of only a finite number of values from some alphabet, such as letters or digits. An example is a text document, which consists of a string of alphanumeric characters. The most common form of digital data in modern information systems is binary data, which is represented by a string of binary digits (bits) each of which can have one of two values, either 0 or 1.
Digital data can be contrasted with analog data, which is represented by a value from a continuous range of real numbers. Analog data is transmitted by an analog signal, which not only takes on continuous values but can vary continuously with time, a continuous real-valued function of time. An example is the air pressure variation in a sound wave.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Data requires interpretation to become information. In modern (post-1960) computer systems, all data is digital. Digital data also serves as the main input for data science, where data is collected, processed, analyzed, and modeled using methods from statistics, computer science and machine learning for pattern recognition and support decision-making.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_**Data at rest**_ in information technology means data that is housed physically on computer data storage in any digital form (e.g. cloud storage, file hosting services, databases, data warehouses, spreadsheets, archives, tapes, off-site or cloud backups, mobile devices etc.).
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_**Data in use**_ is an information technology term referring to active data which is stored in a non-persistent digital state or volatile memory, typically in computer random-access memory (RAM), CPU caches, or CPU registers.[19]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_**Data in transit**_, also referred to as data in motion[28] and data in flight,[29] is data en route between source and destination, typically on a computer network.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**_Model_** is a simplified representation of a system or phenomenon.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**_Data model_**(database design context) is a visual or structural plan (for example, an entity-relationship diagram) that represents a given subject area in a projection onto a database with different levels of abstraction.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**_Data model_**(data theory context) is is a mathematical framework that defines how data is structured, stored, and manipulated using formal rules and constraints.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **_Database_** is organized collection of related data accessed through the use of a database management system (DBMS).\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **_DBMS (Database Management System)_** is a software system that allows you to define, create, maintain, control access to a database, and perform **_CRUD_** (Create,Read,Update,Delete) operations on data in the database.

## 2. Database evolution process

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Before the introduction of modern DBMS, data was managed using basic file systems on hard drives. While this approach allowed users to store, retrieve and update files as needed, it came with numerous challenges:

- Data Redundancy: Duplicate entries across files
- Inconsistency: Conflicting or outdated information
- Difficult Access: Manual file search required
- Poor Security: No control over data access
- Lack of Multi-user Support: No support for collaboration
- No Backup/Recovery: Data loss was often permanent

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; First commercial Network DBMS was IDS (Integrated Data Store) (**1964**) by General Electric GE, designed by database pioneer Charles Bachman.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;First commercial Hierarchical DBMS was IMS (Information Management System) (**1968**) by IBM.

#### Relational model

A landmark paper by Codd (**1970**) defined the relational model and nonprocedural ways of querying data in the relational model, and relational databases were born.
According to Code **_Relation_** is a set of tuples and attributes.

![alt text](<images/Relations by Codd.JPG>)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A **relation** consists of a **heading** and a **body**. The **heading** defines a set of **attributes**, each with a name and data type (sometimes called a domain). The number of attributes in this set is the relation's degree or arity. The **body** is a set of **tuples**. A tuple is a collection of n values, where n is the relation's degree, and each value in the tuple corresponds to a unique attribute. The number of tuples in this set is the relation's cardinality.

_**!!!** Don`t mess with **Relationship** in **Entity-Relationship (ER) Model**. In ER Model **Relationship** means link between entities_

_**Constraints**_ - an arbitrary boolean expressions. If all constraints evaluate as true, the database is consistent; otherwise, it is inconsistent. If a change to a database's relvars would leave the database in an inconsistent state, that change is illegal and must not succeed.

_**Candidate key**_ (or simply a key) is the smallest subset of attributes guaranteed to uniquely differentiate each tuple in a relation. Since each tuple in a relation must be unique, every relation necessarily has a key, which may be its complete set of attributes. A relation may have multiple keys, as there may be multiple ways to uniquely differentiate each tuple.
**(** _An attribute may be unique across tuples without being a key. For example, a relation describing a company's employees may have two attributes: ID and Name. Even if no employees currently share a name, if it is possible to eventually hire a new employee with the same name as a current employee, the attribute subset {Name} is not a key. Conversely, if the subset {ID} is a key, this means not only that no employees currently share an ID, but that no employees will ever share an ID._ **)**

_**Foreign key**_ is a subset of attributes A in a relation R1 that corresponds with a key of another relation R2, with the property that the projection of R1 on A is a subset of the projection of R2 on A. In other words, if a tuple in R1 contains values for a foreign key, there must be a corresponding tuple in R2 containing the same values for the corresponding key.

_**Database normalization**_ is the process of structuring a relational database in accordance with a series of normal forms to reduce data redundancy and improve data integrity.\
\_Codd introduced the concept of normalization and what is now known as the [first normal form (1NF)](https://en.wikipedia.org/wiki/First_normal_form) in 1970. Codd went on to define the [second normal form (2NF)](https://en.wikipedia.org/wiki/Second_normal_form) and [third normal form (3NF)](https://en.wikipedia.org/wiki/Third_normal_form) in 1971,and Codd and Raymond F. Boyce defined the [Boyce-Codd normal form (BCNF)](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form) in 1974.

#### Relational DBMS

_In the **1979s**, Relational Software Inc. (founded by Larry Ellison, Bob Miner, and Ed Oates, later renamed Oracle Corporation) released the first commercial relational database management system (RDBMS) called Oracle V2._

| Feature            | Hierarchical Model (DBMS)        | Network Model (DBMS)       | Relational Model (RDBMS)    |
| :----------------- | -------------------------------- | -------------------------- | --------------------------- |
| Structural Shape   | Tree                             | Graph                      | Tables                      |
| Relationship Types | One-to-Many (1:M)                | Many-to-Many (M:N)         | All types (1:1, 1:M, M:N)   |
| Parent Nodes       | Exactly one for each record      | Two or more per record     | No concept of "parents"     |
| Data Access        | Top-down only (from the root)    | Via complex graph paths    | Direct via any column       |
| Query Flexibility  | Very low                         | Low                        | High (dynamic queries)      |
| Structural Changes | Requires rebuilding the database | Complex pointer re-linking | Very simple (ALTER command) |

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_Here is the evolution of database models illustrated through an e-commerce store example (**Customer, Order, and Product**)._

#### Hierarchical Model (Tree)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Data is organized in a strict top-down structure. Every child record can have only **one parent**.

- _Structure:_ Customer ➡️ Order ➡️ Product
- _How it works:_ A product is nested directly inside a specific order.
- _The Problem:_ If two different customers buy the exact same smartphone, the details of that smartphone (name, price, specs) must be **duplicated** inside each order. The system does not allow a product record to have two different "parent" orders.

#### Network Model (Graph)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This model allows a child record to have multiple parents, solving the data duplication issue through physical memory pointers.

- _Structure:_ Customer ➡️ Order ⬅️ Product
- _How it works:_ The Order record acts as a child to both the Customer (who bought it) and the Product (what was bought) simultaneously.
- _The Problem:_ To find data, a program must manually navigate through physical memory address chains (pointers). If you delete a product that is still linked to an order, the physical chain breaks, which can corrupt the entire database.

#### Relational Model (Tables)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;There are no rigid structural paths or physical pointers. Data is stored in separate, independent tables and linked logically using IDs.

- **Customers Table:** Customer_ID, Name
- **Products Table:** Product_ID, Product_Name, Price
- **Orders Table:** Order_ID, Customer_ID, Product_ID, Order_Date

**How it works:**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To see what a customer purchased, you run a dynamic SQL query that joins the tables on the fly:

```sql
SELECT Customers.Name, Products.Product_Name
FROM Orders
JOIN Customers ON Orders.Customer_ID = Customers.Customer_ID
JOIN Products ON Orders.Product_ID = Products.Product_ID;
```

### Transactions

- Early 1970s **(The Need)**: Early database systems like IBM's IMS handled rudimentary online requests, but updates were risky. If a system crashed mid-update, data was left corrupted or inconsistent.
- 1974-1975 **(The Definition)**: Jim Gray and his colleagues at IBM formally defined the "transaction" as an atomic, indivisible logical unit of work.
- 1976-1978 **(ACID & Locking)**: Gray published foundational papers, such as Notes on Data Base Operating Systems (1978) - establishing concurrency control, two-phase locking (2PL) for isolation, and write-ahead logging for recovery.
- 1983 **(The Acronym)**: The famous **_ACID_** acronym (_Atomicity_, _Consistency_, _Isolation_, _Durability_) was officially coined and popularized by Jim Gray and Andreas Reuter to summarize these strict transaction guarantees.
  - _**A - Atomicity**_ : All-or-nothing execution. Every operation in the transaction must succeed, or the entire transaction is rolled back, leaving the database unchanged
  - _**C - Consistency**_: Preservation of rules. A transaction can only transition the database from one valid state to another, strictly maintaining all data integrity constraints, rules, and triggers.
  - _**I - Isolation**_: Independent execution. Concurrent transactions execute without interfering with one another, ensuring that intermediate, uncommitted states remain invisible to other transactions.
  - _**D - Durability**_: Permanent survival. Once a transaction commits, its changes survive permanently in non-volatile storage, even in the event of an immediate system crash or power failure.

- 1983-1985 **(Commercial Adoption)**: Relational databases were shifting from academic experiments into enterprise business applications. Early relational pioneers like Oracle and IBM's newly released DB2 (1983) heavily implemented these exact transaction rules to handle high-stakes financial data.
- 1986 **(SQL Standardization)**: The American National Standards Institute (ANSI) published the first official SQL standard (SQL-86). This standard locked in transactional commands like COMMIT and ROLLBACK as mandatory features for any compliant relational system, making ACID execution the expected baseline across the software industry.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;By the end of the 1980s, any relational database that could not guarantee full ACID compliance was rejected by enterprises for serious production workloads

### NoSQL

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;With the rise of **_Big Data_** (large amounts of data), it was necessary to develop new systems that will manage this data in a more efficient way. This means better scalability, performance, flexibility, working with semi-structured and unstructured data, etc. This was a reason for the developement of NoSQL databases.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; In **2000** Eric Brewer had laid the theoretical groundwork with the [**CAP Theorem**](https://en.wikipedia.org/wiki/CAP_theorem) which states that any distributed system can only choose two out of _Consistency_, _Availability_, and _Partition Tolerance_.

- _**Consistency**_\
  Every read receives the most recent write or an error. Consistency means that all clients see the same data at the same time, no matter which node they connect to. For this to happen, whenever data is written to one node, it must be instantly forwarded or replicated to all the other nodes in the system before the write is deemed ‘successful’. Consistency as defined in the CAP theorem is quite different from the consistency guaranteed in ACID database transactions.
- _**Availability**_\
  Every request received by a non-failing node in the system must result in a response, without the guarantee that it contains the most recent version of the data. This is the definition of availability in CAP theorem as defined by Gilbert and Lynch. Availability as defined in CAP theorem is different from high availability in software architecture.
- _**Partition tolerance**_\
  The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.

![alt text](<images/CAP theorem.JPG>)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _**BASE**_ ( _Basically Available, Soft State, Eventual Consistency_ ) acronym was invented by **Dan Pritchett**, a Technical Fellow at eBay, in **2008**. He officially introduced and defined the term in his landmark article titled [_"BASE: An Acid Alternative"_](https://queue.acm.org/detail.cfm?id=1394128), published in July 2008 by the Association for Computing Machinery (ACM).

- **_Basically Available:_** The system guarantees availability, meaning requests get a response almost every time, even if a node fails.
- **_Soft State:_** The data values can change over time without user input, due to background updates and replication.
- **_Eventual Consistency:_** The system becomes consistent across all replicas after a short delay, once new updates stop.

**1998** The label _**"NoSQL"**_ was first coined in 1998 by Carlo Strozzi for his lightweight, open-source relational database that did not use standard SQL interfaces.

**mid 2000s** Internet giants hit the limits of traditional relational databases while managing massive, unstructured data. Google published a paper on Bigtable in 2006, and Amazon released details on its Dynamo paper in 2007, inspiring a new wave of distributed architectures.

In **2009** popular modern NoSQL solutions like MongoDB and Redis shifting the acronym to mean "Not Only SQL" to handle high-speed, flexible, and unstructured data.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **_NoSQL_** does not have a formal, generally accepted definition. It represents a form of data storage mechanism that is fundamentally different from RDBMS. NoSQL databases initialy provided BASE principles compliance,
stands for **Not Only SQL**. Its mechanism is modeled in such a way that it does not contain tabular relations. The data structure is simple and designed according to specific data types so that
scientists in database field have can choose the architecture that best suits them. Initially, NoSQL DBMS followed BASE features, while RDBMS were mandatory ACID compliant.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The biggest difference between NoSQL and SQL databases is the support or absence of the SQL language. However, over time it has become difficult to establish a strict DBMS affiliation between these two database systems. Some NoSQL databases have added SQL interfaces to facilitate integration into
traditional environments.

According to Martin Fowler four characteristics associated with most NoSQL are:

Absence of schema: database schema describes all possible data and structures in relational
databases. With NoSQL databases, schemas are optional, so it is possible to store data without a
pre-designed schema.

- Non-relational: in relational databases, connections are established between the tables in which
  the data is stored. With NoSQL databases, it is possible to store information combined in one
  record with all the details that belong to it.
- Hardware adaptability: some databases are designed to work with specialized hardware. With
  NoSQL databases, this is not necessary and cheap servers can be used for easy storage capacity
  expansion.
- High distributability: With distributed databases, it is possible to store and process data from
  several machines (servers). With NoSQL databases, server clusters can be used to store data in one
  large database.

#### Vertical and horizontal extensions

**_Vertical extension_** - Upgrading a single machine's hardware capacity (more RAM, faster SSDs, stronger CPU).

_**Horizontal extension**_ (commonly called horizontal scaling or scaling out) means adding more hardware nodes or servers to a database cluster to share the workload, rather than upgrading a single server's CPU or RAM.

Key Benefits of horizontal extention:

- _Cost Efficiency:_ Instead of buying expensive, proprietary enterprise servers, companies can scale out by using affordable commodity hardware or standard cloud instances.
- _Zero Downtime:_ New machines can be plugged into a running cluster dynamically. The NoSQL system automatically redistributes the data load without taking the database offline.
- _Fault Tolerance:_ Because data is replicated, the system easily survives individual hardware crashes without interrupting the application

##### RDBMS - horisontal extension.

Relational databases are struggle to scale out because of:

- _**ACID Compliance:**_ Maintaining strong consistency across multiple physical machines requires distributed locks and consensus protocols, which can severely slow down performance.
- _**Complex JOIN Operations:**_ If Table A lives on Node 1 and Table B lives on Node 2, running an SQL JOIN requires moving massive amounts of data over the network, introducing heavy latency.

The ways to provide horizontal extension ro RDBMS:

- **_Read Replicas_** (Primary-Replica Topology)
  - _How it works:_ One central server (the Primary) handles all data writes and updates. It copies the data to multiple secondary servers (Replicas).
  - _Best use case:_ Applications with heavy read traffic but light write traffic (e.g., blogs, e-commerce product catalogs).
  - _Limitation:_ It does not scale write capacity, as all writes must still go through the single primary node.
- **_Database Sharding_**
  - _How it works:_ Breaking a massive table down into smaller pieces (shards) based on a specific "sharding key" (like Customer ID or Region). Each shard resides on a completely different server node.
  - _Best use case:_ Massively scalable web applications handling huge volumes of structured data.
  - _Limitation:_ Highly complex to implement and maintain. Cross-shard joins or transactions become highly inefficient.
- **_NewSQL / Distributed RDBMS_**
  - _How it works:_ A newer generation of relational databases built from scratch with a cloud-native, distributed architecture. They split data into small consensus groups using algorithms like Raft or Paxos, handling ACID compliance natively across many machines.
  - _Best use case:_ Enterprise systems requiring absolute data accuracy alongside seamless horizontal elasticity.

| Approach            | Scalability Type | Complexity        | Key Feature          | Example Tools                      |
| :------------------ | ---------------- | ----------------- | -------------------- | ---------------------------------- |
| **Read Replicas**   | Reads only       | Low               | Simple setup         | Standard MySQL, PostgreSQL         |
| **Manual Sharding** | Reads & Writes   | High              | App-level management | Citus (Postgres extension), Vitess |
| **Distributed SQL** | Reads & Writes   | Medium (Built-in) | Native cloud scaling | CockroachDB, Google Spanner        |

##### NoSQL - horizontal extension.

NoSQL databases are suitable for horizontal expansion because:

- **_Flexible Schemas_**. No rigid structures or complex cross-table relationships (JOINs), making data chunks highly independent and easy to move.
- **_Eventual Consistency_** Many NoSQL tools use the BASE model rather than strict ACID. They prioritize immediate performance and assume data updates will sync across all nodes within milliseconds.- **_Peer-to-Peer Topologies_** Masterless or leaderless node setups mean there is no single master bottleneck or point of failure for write operations.

#### DBMS components

![alt text](<images/DBMS components.jpg>)

##### Hardware

- Physical devices like servers, disks, input-output devices (keyboard, monitor, printer).
- Stores and processes data; interfaces between real-world inputs and digital systems.
- Examples: Personal computer hard disk, RAM, network devices used for DBMS operations.

##### Software

- Actual DBMS software like MySQL, Oracle, PostgreSQL.
- Includes the database engine(Query Processor, Storage Manager), OS, network software, and application tools.
- Translates database access languages into operations.

##### Data

- Raw facts stored in structured or unstructured formats. On its own, raw data lacks context or a specific meaning, but when processed, structured, and organized by a DBMS, it transforms into meaningful
- **_Operational Data:_** Actual user data (e.g., name, age).
- **_Metadata:_** Data about data (e.g., storage time, size, data type).
- Core reason DBMS exists—to manage and store data efficiently.

##### Procedures

- Instructions and rules for using DBMS effectively.
- Covers setup, login/logout, data validation, backup, access control, and report generation.
- Helps ensure consistent and secure use of the system.

##### Database Access Language

- Used to interact with the database (create, read, update, delete data).
- Examples: SQL, MyAccess, Oracle PL/SQL.

##### People

People interacting with DBMS at different levels:

- **_Database Administrators (DBA)_** - Manage security, performance, user access.
- **_Developers_** - Build applications using the database.
- **_End Users_** - Use applications to access the database (e.g., students, employees).

### DBMS types

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Databases and DBMSs can be categorized according to the database model(s)() that they support (such as relational or XML), the type(s) of computer they run on (from a server cluster to a mobile phone), the query language(s) used to access the database (such as SQL or XQuery), and their internal engineering, which affects performance, scalability, resilience, and security.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This is a list of criteria used for classifying different types of DBMS:

- Data model(data theory context): How the data stored in the DBMS is structured and organized.
- Access method: Whether data can be accessed with SQL or NoSQL.
- Data consistency model: Whether the DBMS follows strong consistency (ACID), eventual consistency (BASE), or more. The differences between the two are outlined in an AWS blog.
- Use cases: Typical usage scenarios.
- Examples: List of real-world databases under the particular type of DBMS

_**BASE** (Basically Available, Soft state, Eventually consistent)_\
_**ACID** (Atomicity, Consistency, Isolation, Durability)_

##### DBMS overview

##### Hierarchical Database Management System (Hierarchical DBMS)

_**Hierarchical databases**_ organize data in a tree-like structure, primarily used for fast, structured navigation and efficient data retrieval, especially in applications where relationships are modeled as parent-child hierarchies.\

**Characteristics:**

- _Data model:_ Hierarchical (tree-based)
- _Access method:_ SQL-like query languages
- _Data consistency model:_ Mostly strong consistency (ACID)
- _Use cases:_ Mainframe applications, directory services, legacy banking systems
- _Examples:_ IBM IMS, Windows Registry

**Advantages:**

- _Simple Structure:_ It’s easy to understand and navigate.
- _Strong Data Integrity:_ Keeps your data accurate and consistent.
- _Quick Access:_ Fast retrieval thanks to its tree structure.
- _Good Security:_ Simple to control access at different levels.
- _Easy Recovery:_ Straightforward to roll back or restore data.

_**Disadvantages:**_

- _Rigid Structure:_ Not great for handling complex relationships.
- _Scalability Issues:_ Gets cumbersome as data grows and diversifies.
- _Data Redundancy:_ Duplication issues can arise within branches.
- _Limited Flexibility:_ Tough to adapt or modify once set up.
- _Access Limitations:_ Traversing data can be slow outside primary paths.

##### Network Database Management System (Network DBMS)

**_Network databases_** organize data as a flexible collection of relationships, where each record can have multiple parent and child records, forming a more complex structure than the hierarchical model. They can be a solution for complex applications, making it easier to handle and connect data that’s all tangled up in different ways.

**Characteristics:**

- _Data model:_ Graph-based
- _Access method:_ Relies on pointers to navigate between records
- _Data consistency model:_ Typically follows ACID principles, providing strong consistency, though some implementations may opt for eventual consistency in distributed environments.
- _Use cases:_ Supply chain management, insurance applications
- _Examples:_ TurboIMAGE, Integrated Data Store (IDS), Raima Database Manager.

**Advantages**

- Flexible Structure: Adapts easily to complex relationships between data.
- Improved Data Access: More efficient data querying than hierarchical databases.
- Data Integrity: Maintains high data integrity with less redundancy.

**Disadvantages**

- Complex to Design: More challenging to set up.
- High Maintenance: Requires more effort to manage and update.
- Skill Requirement: Demands a high level of skill to navigate effectively.

##### Relational Database Management System (RDBMS)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**_Relational DBMS_** store data in tables, which are organized into rows and columns. Each table represents a different type of entity, and relationships between tables are defined through foreign keys. This setup is perfect for apps that need tight data. An RDBMS organizes data into structured tables that follow predefined schemas. Relational database management systems use SQL to access, manage, and manipulate data.
Also, they generally follow the ACID (Atomicity, Consistency, Isolation, Durability) principles to guarantee strong consistency, making them ideal for any transaction-based application.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Also, they generally follow the ACID (Atomicity, Consistency, Isolation, Durability) principles to guarantee strong consistency, making them ideal for any transaction-based application.\

**Example:**\
Oracle Database, MySQL, PostgreSQL, MSSQL e.t.c\

**Characteristics:**

- _Data model:_ Structured, table-based
- _Access method:_ SQL
- _Data consistency model:_ Strong consistency (ACID) -_ Use cases:_ Financial systems, ERP, CRM, transactional applications, and more
- _Examples:_ MySQL, PostgreSQL, Oracle Database, Microsoft SQL Server, SQLite, IBM Db2

**Advantages:**

- Strict ACID compliance ensures safe, reliable transactions.
- Complex Queries: Powerful query capabilities using SQL
- Minimal data duplication, data integrity and consistency maintenance

**Disadvantages:**

- Schema Rigidity: Requires pre-defined data structures hard to alter once set.
- Complexity: Managing relationships and schema may be complex while scaling.
- Performance: Can suffer performance issues when scaling horizontally.
- Cost: High operational costs for large-scale implementations.
- Resources: High transaction volumes can consume significant resources.

##### Object-Oriented Database Management System (OODBMS)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Object-oriented databases store data in objects, similar to object-oriented programming, allowing data to be stored in complex structures that represent both state and behavior. These databases mesh really well with object-oriented programming languages, which makes them a good fit for handling complex data setups.

**Example:**\
ObjectStore, db4o

**Characteristics:**

- _Data model:_ Object-based
- _Access method:_ Object query languages (OQL)
- _Data consistency model:_ In most cases, strong consistency (ACID)
- _Use cases:_ Any application that involves storing and managing graphs of objects, or objects in general, such as inventory and CAD/CAM applications and Asset management systems.

**Advantages:**

- Direct Mapping: They align with object-oriented programming concepts.
- Data Encapsulation: Ensure high level of data integrity.
- Flexibility: Diverse data type and structure management.
- Complex Queries: Supports complex queries effectively due to object relationships.

**Disadvantages**

- Complexity: Can be complex to design and manage.
- Performance Issues: May experience slower performance with large data volumes.
- Limited Tools: Fewer tools and less community support compared to other systems.
- Learning Curve: Requires a good understanding of object-oriented concepts.

##### Non-Relational (NoSQL Database Management System (NOSQL DBMS))

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**_NoSQL DBMS_** is designed to handle large volumes of unstructured, semi-structured, or even structured data while ensuring high flexibility and performance. Unlike RDBMS, a NoSQL database management system does not rely on fixed schemas and generally follows the BASE (Basically Available, Soft-state, Eventual consistency) model for data consistency.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;In this context, "NoSQL" stands for "Not Only SQL," which means that these databases are not limited to traditional SQL-based relational models.

**Characteristics:**

- _Data model:_ Key-value, document, column-family, or graph
- _Access method:_ NoSQL
- _Data consistency model:_ Eventual consistency (BASE), with some offer strong consistency
- _Use cases:_ Social networks, content management, IoT, real-time analytics
  _Examples:_ MongoDB, Cassandra, Redis, Amazon DynamoDB, Neo4j

**Advantages:**

- Flexibility: Easily accommodates unstructured and semi-structured data.
- Scalability: Great at horizontal scaling and managing big data.
- Speed: Provides faster queries for certain types of data and applications.
- Schema-less: Allows on-the-fly modifications to structures without downtime.
- Diverse Data Types: Supports a variety of data formats within a single system.

**Disadvantages**

- Consistency: May sacrifice ACID properties for speed and flexibility.
- Complex Queries: Less efficient at handling complex queries.
- Management Complexity: Lack of universal query language and standards.
- Data Integrity: More complicated to ensure data integrity.
- Specialized Skills Required: Often requires specific knowledge and architectures.

NoSQL databases are categorized according to they store data model:

- _**Key-Value Databases**_
  Key-value databases are the sprinters of the data world, optimized for swift look-ups by using unique keys. They shine in scenarios requiring high-speed access for large volumes of data, like session management and caching. Long story short, they’re about getting you data fast. (e.g., Redis and Riak).
- **_Document Databases_**
  Document databases store data in JSON-like formats, making them a hit for developers looking to keep their data structure. Super intuitive for storing, retrieving, and managing document-oriented information, these databases are ideal for content management systems and user profiles. (e.g., MongoDB and CouchDB).
- **_Columnar Databases_**
  Columnar databases store data in columns instead of rows, and they serve analytics best. This setup allows for faster retrieval of data, efficient data compression, and better disk I/O. They’re a favorite for data warehousing and big data processing, where operations often involve large amounts of similar data. (e.g., Cassandra and HBase). Learn more about columnar databases.
- **_Wide Column Databases_**
  Wide column databases handle enormous amounts of data but also allow each row to have a different set of columns. This makes them incredibly versatile and scalable, perfect for real-time analysis across diverse and voluminous datasets.
- **_Graph Databases_**
  Graph databases store data in nodes and edges, which represent entities and their interrelationships, respectively. Perfect for analyzing networks like social connections, logistics networks, or even complex dependencies in data, they offer the ability to traverse vast webs of information quickly and with precision. (e.g., Neo4j and ArangoDB).
- **_Time Series Databases_**
  Time series databases are specialized in handling sequences of data points indexed in time order like stock market trends, energy usage monitoring, or any metric that changes over time. They’re optimized to store, retrieve, and process time-based data efficiently, making them indispensable for real-time analytics in dynamic environments.

##### NewSQL Database Management System (NewSQL DBMS)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A NewSQL DBMS aims to combine the scalability and high availability of NoSQL systems in online transaction processing (OLTP), with the ACID guarantees of traditional RDBMS. These databases are optimized for high-performance applications while guaranteeing data integrity.

**Characteristics:**

- _Data model:_ Relational (table-based)
- _Access method:_ SQL
- _Data consistency model:_ Strong consistency (ACID)
- _Use cases:_ Applications characterized by heavy OLTP (Online Transaction Processing) transaction volumes such as financial transactions or real-time data analysis systems
- _Examples:_ Google Spanner, CockroachDB, TiDB

**Advantages**

- _ACID Compliance:_ They maintain strict data integrity and strong consistency for online transaction processing (OLTP).
- _Horizontal Scalability:_ They scale out across multiple servers or clusters without hitting the single-node bottlenecks of traditional SQL systems.
- _SQL Compatibility:_ They support familiar SQL queries and relational models, making it easy for developers to migrate existing applications.
- _High Availability:_ They feature built-in fault tolerance, automatic sharding, and replication to ensure continuous operation during hardware failures.

**Disadvantages**

- _Operational Complexity:_ Managing distributed nodes, consensus algorithms (like Raft or Paxos), and network partitions requires specialized expertise.
- _Higher Latency for Distributed Transactions:_ Multi-shard or cross-node transactions can introduce latency compared to single-node traditional databases.
- _Resource Costs:_ Running a distributed, highly available NewSQL architecture can be expensive and resource-heavy.
- _Rigid Schemas:_ Unlike flexible NoSQL document stores, they generally require predefined schemas, limiting how easily unstructured data is handled.

#### For which projects is it better to use NoSQL or relational databases, respectively?

**_When to Use Relational DBMS_**

- _Strict Consistency & ACID Transactions:_ Choose relational databases like PostgreSQL or MySQL when operations require absolute data integrity, such as financial transactions, payments, and orders.
- _Complex Relationships & Joins:_ Best when your data splits cleanly into normalized tables that require complex multi-table queries and joins.
- _Stable Schemas:_ Ideal when the data structure is well-defined and unlikely to change frequently.

**_When to Use NoSQL DBMS_**

- _Unstructured or Rapidly Changing Data:_ Choose NoSQL options like MongoDB or Cassandra when storing documents, user profiles, or logs with evolving attributes that do not fit a rigid table.
- _Massive Horizontal Scalability:_ Best when your app needs to scale out across multiple servers or regions easily to handle high write/read volumes (like IoT data or real-time feeds).
- _High-Throughput Operations:_ Perfect for use cases prioritizing speed and low latency over strict relational integrity, such as messaging apps or caching layers.

## 4. Data modeling

#### Data Model (database design context)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Data model is a visual blueprint that shows how data is organized, stored, and connected within database. It defines defines how data elements relate to each other and to real-world entities.

#### Data models purposes:

- Organization: They prevent data chaos by establishing clear structure and rules
- Consistency: They ensure everyone uses the same definitions and formats
- Efficiency: Well-designed models make data retrieval and analysis faster
- Communication: They provide a common language for discussing data requirements
- Quality: They help prevent errors and inconsistencies in data storage

#### Classification of data models by levels of detail and abstraction::

- **_Conceptual data model:_** High-level view focusing on what data exists and how it relates, without technical details. These are often used for initial planning and communication with business stakeholders.

- **_Logical data model:_** is a detailed, vendor-agnostic blueprint that structures data elements and their operational boundaries based on a pre-selected database paradigm (Relational or NoSQL).While it remains independent of specific software vendors (like Oracle vs. PostgreSQL, or MongoDB vs. DynamoDB), it is deeply dependent on the data architecture paradigm chosen to solve the business problem.

- **_Physical data model:_** is a highly detailed, technology-specific blueprint that represents how data will be actualized, organized, and physically stored inside a specific Database Management System (DBMS).It is the final stage of data modeling, transforming the conceptual and logical designs into executable code and concrete storage structures tailored to a chosen vendor (such as PostgreSQL, Oracle, MongoDB, or AWS DynamoDB).

#### Effective data models share common characteristics:

- _Clarity:_ Easy to understand and explain to both technical and business stakeholders
- _Flexibility:_ Able to accommodate future changes and growth
- _Efficiency:_ Optimized for the most common ways data will be accessed and used
- _Accuracy:_ Correctly represent real-world relationships and business rules
  Simplicity: As simple as possible while meeting all requirements

Common Challenges
Data modeling can present several challenges:

Changing requirements: Business needs evolve, requiring model updates
Performance trade-offs: Models optimized for storage may not be best for analysis
Legacy constraints: Existing systems may limit modeling options
Stakeholder alignment: Different groups may have conflicting data needs
Complexity management: Balancing completeness with usability

Data Models vs. Other Concepts
It's helpful to distinguish data models from related concepts:

Data models vs. databases: The model is the plan; the database is the implementation of that plan.

Data models vs. data architecture: Models focus on structure; architecture includes broader technical decisions about storage, processing, and access.

Data models vs. schemas: Schemas are technical implementations of logical data models in specific database systems.

Impact on Business Success
Well-designed data models contribute to business success by:

Enabling better decisions: Consistent, organized data supports accurate analysis
Improving efficiency: Faster data access and reduced errors
Supporting growth: Flexible models accommodate new requirements
Ensuring compliance: Proper models help meet regulatory requirements
Reducing costs: Fewer data quality issues and system problems

[Data modeling](https://tdwi.org/blogs/data-101/2025/09/what-is-a-data-model.aspx)

Data models are fundamental to organizing and using information effectively. Whether you're managing a small business database or designing enterprise systems, understanding data models helps you think clearly about information structure and create systems that truly serve user needs. Good data models are invisible to end users but essential for system success—they're the foundation that makes everything else possible.
