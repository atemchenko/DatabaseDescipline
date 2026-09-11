## DDL

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Data Definition Language (DDL) consists of SQL commands used to define, modify, and manage the structure of a database and its objects (such as databases, tables, indexes, and views). Unlike DML (Data Manipulation Language), which handles the data inside the tables, DDL changes the blueprint of the schema.\
In modern versions like MySQL 8.x, DDL statements are atomic, meaning the schema changes are completely committed or rolled back if an error occurs.

### Database Operations

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Before creating tables, you must manage the database container itself.

- Create a new database
  > CREATE DATABASE \<database name>;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if database with name myFirstDb already exists you will receive an error.
To avoid error use option _**IF NOT EXISTS**_

> CREATE DATABASE IF NOT EXISTS \<database name>;

[CREATE DATABASE tutorial](https://dev.mysql.com/doc/refman/9.7/en/create-database.html)

- Switch to the database context.

  > USE \<database name>

- Delete a database (Permanently removes all data and tables!)

  > DROP DATABASE \<database name>;

- Change database options\
  To change database options use [ALTER DATABASE](https://dev.mysql.com/doc/refman/9.7/en/alter-database.html)

### Table Operations

- Create new table\
  New tables are added to an existing database using the CREATE TABLE statement.

```sql
CREATE TABLE customer
(
  customer_id int NOT NULL,
  customer_name char(20) NOT NULL,
  customer_address char(20) NULL,
  PRIMARY KEY (customer_id)
);

CREATE TABLE orders
(
  order_id int NOT NULL,
  order_name char(20) NOT NULL,
  order_address char(20) NULL,
  customer_id int NOT NULL,
  PRIMARY KEY (order_id),
  FOREIGN KEY (customer_Id) references customer(customer_id)
);
```

[CREATE TABLE tutorial](https://dev.mysql.com/doc/refman/9.7/en/create-table.html)

- Displaying table schema

To display table schema use [DESCRIBE](https://dev.mysql.com/doc/refman/9.7/en/describe.html)

> DESCRIBE \<table name>

- Delete table

To delete the sales table from the database, use the following command:

> DROP TABLE \<table name>;

Alternatively, use the TRUNCATE TABLE statement to permanently remove all rows from a table without deleting the table itself:

> TRUNCATE TABLE \<table name>;

### MySQL storage engine types

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MySQL comes with several storage engines, each offering specific advantages. Using the _**ENGINE=**_ directive, you can choose the storage engine for each table individually. Some of the storage engines currently available in MySQL include:

- _**InnoDB**_ \
  The InnoDB storage engine was introduced with MySQL version 4.0 and is known for being transaction-safe. A transaction-safe storage engine guarantees that all database transactions are fully completed and will roll back any partially completed transactions (for example, as a result of a server or power failure). This ensures that a database is never left with incomplete data updates.
- _**MEMORY**_ \
  The MEMORY storage engine stores data in memory rather than on disk. This makes the engine extremely fast. However, the transient nature of data in memory means this engine is more suitable for temporary table storage.
- _**CSV**_
  The CSV storage engine saves data in plain text files using a comma-separated values (CSV) format. This engine is particularly useful when the stored data needs to be exchanged with other platforms, such as spreadsheets or accounting software.
- _**ARCHIVE**_ \
  The ARCHIVE engine utilizes zlib compression to minimize the storage space required for large data volumes.

To generate a list of supported engines, use the following _**SHOW ENGINES**_ statement:\

> SHOW ENGINES\G

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Different engine types can be used within a database; for example, some tables may utilize the InnoDB engine, while others might use the CSV engine. If an engine type is not specified when creating a table, MySQL will default to using the InnoDB engine for that table. To identify the default engine, you can use the following statement:

> SELECT @@default_storage_engine;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To specify a particular engine type for a table, add the appropriate ENGINE= definition after defining the table columns. For instance, the following example specifies the MEMORY engine:

```sql
CREATE TABLE customer
(
  customer_id int,
  customer_name char(20) NOT NULL,
  customer_address char(20) NULL,
  PRIMARY KEY (customer_id)
)ENGINE=MEMORY;
```
