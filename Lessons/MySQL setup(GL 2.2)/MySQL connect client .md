### MySQL client

The MySQL client is a command-line tool installed with the MySQL package bundle. It can also be installed separately on non-server systems. From the MySQL shell prompt, you can execute various commands on both local and remote database servers. This includes tasks such as creating and deleting databases and tables, searching for data, adding new rows, and much more.

#### Connect to MySQL server

Open command line and enter\

> mysql -u \<username> -p

By running _**mysql -u root -p**_, you are launching the MySQL command-line client, and that client is actively attempting to establish a live connection to the MySQL server.Here is exactly how it works under the hood:

- Launching the ClientThe mysql command initializes the MySQL command-line client tool. The flags you provided tell the client how to authenticate.\
  **_-u root_**: Specifies that you want to log in as the root user database account.\
  **_-p_**: Tells the client to prompt you for the password.
- Connecting to the ServerThe moment you enter this command and successfully type your password, the client uses those credentials to connect to the MySQL server instance (by default, running on localhost via port 3306 or a local Unix socket).

If you need connect to not default server use:

> mysql -h \<remote server> -P \<port number> -u \<username> -p

#### Using the mysql client

Once the client is running, you will see the **_mysql>_** prompt where you can enter commands and statements. SQL statements typed at this prompt are executed interactively, and the results are shown in real-time. For example, the following statement will display a list of databases managed by the server:

> mysql> SHOW DATABASES;

_**!** SQL statements must end with a semicolon (;) or \G. If a command is not terminated, pressing Enter continues the current statement on the next line_

SQL statements are not case-sensitive when used in the MySQL client. he following statements, for example, are interpreted identically:\
SELECT _ FROM MYSQL.USER;\
select _ from mysql.user;\
Select \* From Mysql.User;

Detailed information about MySQL command tool you can find in
[MySQL command line tutorial](https://dev.mysql.com/doc/refman/9.7/en/mysql.html)
