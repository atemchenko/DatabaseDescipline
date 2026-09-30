## DDL

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Data Definition Language (DDL) consists of SQL commands used to define, modify, and manage the structure of a database and its objects (such as databases, tables, indexes, and views). Unlike DML (Data Manipulation Language), which handles the data inside the tables, DDL changes the blueprint of the schema.\
In modern versions like MySQL 8.x, DDL statements are atomic, meaning the schema changes are completely committed or rolled back if an error occurs.

### Database Operations

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Before creating tables, you must manage the database container itself.

- Create a new database
  > CREATE DATABASE \<database name>;

  if database with name myFirstDb already exists you will receive an error.
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

- Review create table script

>SHOW CREATE TABLE \<table name>

- Review table constraints 

```sql
SELECT 
    CONSTRAINT_NAME, 
    CONSTRAINT_TYPE 
FROM 
    information_schema.TABLE_CONSTRAINTS 
WHERE 
    TABLE_SCHEMA = '\<database name>'
    AND TABLE_NAME = '\<table name>'
```

- Displaying table schema

To display table schema use [DESCRIBE](https://dev.mysql.com/doc/refman/9.7/en/describe.html)

> DESCRIBE \<table name>

- Delete table

To delete the sales table from the database, use the following command:

> DROP TABLE \<table name>;

Alternatively, use the TRUNCATE TABLE statement to permanently remove all rows from a table without deleting the table itself:

> TRUNCATE TABLE \<table name>;

- Alter table

_**ALTER TABLE**_ is used to modify an existing table, namely:\
&nbsp; - adding, deleting columns;\
&nbsp; - add, drop constraints;\
&nbsp; - rename columns;

_syntax_:

```sql
ALTER TABLE table_name
AFTER_ACTION
```

Drop _**first_name**_ column from the _**staff**_ table.

```sql
ALTER TABLE staff
DROP COLUMN first_name;
```

Add _**date_of_birth**_ column to the _**staff**_ table

```sql
ALTER TABLE staff
ADD COLUMN date_of_birth DATE;
```

Change column _**address_id**_ type in the _**staff**_ table.

```sql
ALTER TABLE staff
ALTER COLUMN address_id TYPE SMALLINT;
```

Rename column _**first_name**_ to _**name**_ in the _**staff**_ table.

```sql
ALTER TABLE staff
RENAME COLUMN first_name TO name;
```

for legacy versions (better for backward compatibility)

```sql
ALTER TABLE staff
CHANGE COLUMN first_name name VARCHAR(50);
```

Add NOT NULL constraint for store_id column in the staff table.

```sql
ALTER TABLE staff
MODIFY COLUMN store_id SMALLINT NOT NULL;
```

**add constraint**
```sql
ALTER TABLE <table name>
     add constraint <fk name>  foreign key (<column name>)   REFERENCES <referenced table name> (<referenced table column>) ON UPDATE {CASCADE|NO ACTION|RESTRICT|SET} ON DELETE {CASCADE|NO ACTION|RESTRICT|SET};
```
_example:_

Add a foreign key to the _**orders**_ table references the primary key customer_id in the **_customer_** table. 

```sql
alter table orders
add constraint fk_order_customer foreign key (customer_id) references customer(customer_id) on update cascade on delete restrict;
```

**drop constraint**

```sql
ALTER TABLE <table name>
     drop constraint <constraint name>;
```
[ALTER TABLE full tutorial](https://dev.mysql.com/doc/refman/8.0/en/alter-table.html)

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
