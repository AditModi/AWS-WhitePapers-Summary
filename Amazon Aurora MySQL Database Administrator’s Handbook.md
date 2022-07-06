# Amazon Aurora MySQL Database Administrator’s Handbook

- This paper outlines the best practices for managing database connections, setting server connection parameters, and configuring client programs, drivers, and connectors. 
- 

- It’s a recommended read for Amazon Aurora MySQL Database Administrators (DBAs) and application developers.

## Introduction

- Amazon Aurora MySQL (Aurora MySQL) is a managed relational database engine, wire-compatible with MySQL 5.6 and 5.7. Most of the drivers, connectors, and tools that you currently use with MySQL can be used with Aurora MySQL with little or no change.

- Aurora MySQL database (DB) clusters provide advanced features such as:

    - One primary instance that supports read/write operations and up to 15 Aurora Replicas that support read-only operations. 
    
    - Each of the Replicas can be automatically promoted to the primary role if the current primary instance fails.

    - A cluster endpoint that automatically follows the primary instance in case of failover.
    
    - A reader endpoint that includes all Aurora Replicas and is automatically updated when Aurora Replicas are added or removed.
    
    - Ability to create custom DNS endpoints containing a user-configured group of database instances within a single cluster.
    
    - Internal server connection pooling and thread multiplexing for improved scalability. 


## DNS endpoints

- An Aurora DB cluster consists of one or more instances and a cluster volume that manages the data for those instances. 

- There are two types of instances:

  - Primary instance – Supports read and write statements. Currently, there can be one primary instance per DB cluster.

  - Aurora Replica – Supports read-only statements. 
    - A DB cluster can have up to 15 Aurora Replicas. 
    - The Aurora Replicas can be used for read scaling, and are automatically used as failover targets in case of a primary instance failure.


- Amazon Aurora supports the following types of Domain Name System (DNS) endpoints:
  
  - Cluster endpoint – Connects you to the primary instance and automatically follows the primary instance in case of failover, that is, when the current primary instance is demoted and one of the Aurora Replicas is promoted in its place.

  - Reader endpoint – Includes all Aurora Replicas in the DB cluster under a single DNS CNAME. You can use the reader endpoint to implement DNS round robin load balancing for read-only connections.

  - Instance endpoint – Each instance in the DB cluster has its own individual endpoint. You can use this endpoint to connect directly to a specific instance.

  - Custom endpoints – User-defined DNS endpoints containing a selected group of instances from a given cluster.


## Connection handling in Aurora MySQL and MySQL

- MySQL Community Edition manages connections in a one-thread-per-connection fashion. 
- This means that each individual user connection receives a dedicated operating system thread in the mysqld process. 
- Issues with this type of connection handling include:

  - Relatively high memory use when there is a large number of user connections, even if the connections are completely idle. 
  - Higher internal server contention and context switching overhead when working with thousands of user connections. 




## Common misconceptions

The following are common misconceptions for database connection management: 

 - If the server uses connection pooling, you don’t need a pool on the application side. This isn’t true for workloads where connections are opened and torn down very frequently, and clients run relatively few statements per connection.
 
 - Idle connections don’t use memory. This isn’t true because the operating system and the database process both allocate an in-memory descriptor for each user connection. What is typically true is that Aurora MySQL uses less memory than MySQL Community Edition to maintain the same number of connections. However, memory usage for idle connections is still not zero, even with Aurora MySQL.

 - Downtime depends entirely on database stability and database features. This isn’t true because the application design and configuration play an important role in determining how fast user traffic can recover following a database event.


## Best practices

The following are best practices for managing database connections and configuring connection drivers and pools.

### Using smart drivers

The cluster and reader endpoints abstract the role changes and topology changes occurring in the DB cluster. However, DNS updates are not instantaneous. 

In addition, they can sometimes contribute to a slightly longer delay between the time a database event occurs and the time it’s noticed and handled by the application.


Aurora MySQL exposes near-real-time metadata about DB instances in the INFORMATION_SCHEMA.REPLICA_HOST_STATUS table.

```sql
  
mysql> select server_id, if(session_id = 'MASTER_SESSION_ID',
'writer', 'reader') as role, replica_lag_in_milliseconds from
information_schema.replica_host_status;

+-------------------+--------+-----------------------------+
| server_id | role | replica_lag_in_milliseconds |
+-------------------+--------+-----------------------------+
| aurora-node-usw2a | writer | 0 |
| aurora-node-usw2b | reader | 19.253999710083008 |
+-------------------+--------+-----------------------------+
2 rows in set (0.00 sec)

```


For the purpose of this whitepaper, a smart driver is a database driver or connector with the ability to read DB cluster topology from the metadata table. 

It can route new connections to individual instance endpoints without relying on high-level cluster endpoints. 

A smart driver is also typically capable of load balancing read-only connections across the available Aurora Replicas in a round robin fashion.


The MariaDB Connector/J is an example of a third-party Java Database Connectivity (JDBC) smart driver with native support for Aurora MySQL DB clusters. 

Application developers can draw inspiration from the MariaDB driver to build drivers and connectors for languages other than Java.


It’s a good idea to evaluate the use of smart drivers in your setup. 

Note that if a thirdparty driver contains Aurora MySQL–specific functionality, it doesn’t mean that it has been officially tested, validated, or certified by AWS. 

Also note that due to the advanced built-in features and higher overall complexity, smart drivers are likely to receive updates and bug fixes more frequently than traditional (bare bones) drivers. 


You should regularly review the driver’s release notes and use the latest available version whenever possible.


### DNS caching

Unless you use a smart database driver, you depend on DNS record updates and DNS propagation for failovers, instance scaling, and load balancing across Aurora Replicas.

Currently, Aurora DNS zones use a short Time-To-Live (TTL) of five seconds. 

Ensure that your network and client configurations don’t further increase the DNS cache TTL. 

Remember that DNS caching can occur anywhere from your network layer, through the operating system, to the application container. 

For example, Java virtual machines (JVMs) are notorious for caching DNS indefinitely unless configured otherwise. 

Here are some examples of issues that can occur if you don’t follow DNS caching best practices:


- After a new primary instance is promoted during a failover, applications continue to send write traffic to the old instance. 

- Data-modifying statements will fail because that instance is no longer the primary instance.

- After a DB instance is scaled up or down, applications are unable to connect to it. 

- Due to DNS caching, applications continue to use the old IP address of that instance, which is no longer valid.

- Aurora Replicas can experience unequal utilization, for example, one DB instance receiving significantly more traffic than the others.


### Connection management and pooling

Always close database connections explicitly instead of relying on the development framework or language destructors to do it. 

There are situations, especially in container based or code-as-a-service scenarios, when the underlying code container isn’t immediately destroyed after the code completes. 

In such cases, you might experience database connection leaks where connections are left open and continue to hold resources (for example, memory and locks).

### Connection scaling

The most common technique for scaling web service capacity is to add or remove application instances in response to changes in user traffic. 

Each application server can use a database connection pool.

This approach causes the total number of database connections to grow proportionally with the number of application instances. 

For example, 20 application servers configured with 200 database connections each would require a total of 4,000 database connections. 

If the application pool scales up to 200 instances (for example, during peak hours), the total connection count will reach 40,000. Under a typical web application workload, most of these connections are likely idle. 

In extreme cases, this can limit database scalability: idle connections do take server resources, and you’re opening significantly more of them than you need.

Also, the total number of connections is not easy to control because it’s not something you configure directly, but rather depends on the number of application servers.


- Tune the connection pools on application instances. Reduce the number of connections in the pool to the acceptable minimum. 
- This can be a stop-gap  solution, but it might not be a long-term solution as your application server fleet continues to grow.

- Introduce a connection proxy between the database and the application. 
- On one side, the proxy connects to the database with a fixed number of connections. 
- On the other side, the proxy accepts application connections and can provide additional features such as query caching, connection buffering, query rewriting/routing, and load balancing.


### Transaction management and autocommit

With autocommit enabled, each SQL statement runs within its own transaction. 

When the statement ends, the transaction ends as well. Between statements, the client connection is not in transaction. 

If you need a transaction to remain open for more than one statement, you explicitly begin the transaction, run the statements, and then commit or roll back the transaction.

With autocommit disabled, the connection is always in transaction. 

You can commit or roll back the current transaction, at which point the server immediately opens a new one.

Running with autocommit disabled is not recommended because it encourages longrunning transactions where they’re not needed. 

##### Recommendations:

- Always run with autocommit mode enabled. Set the autocommit parameter to 1 on the database side (which is the default) and on the application side (which might not be the default).

- Always double-check the autocommit settings on the application side. For example, Python drivers such as MySQLdb and PyMySQL disable autocommit by default.

- Manage transactions explicitly by using BEGIN/START TRANSACTION and COMMIT/ROLLBACK statements. You should start transactions when you need them and commit as soon as the transactional work is done.



### Connection handshakes

A lot of work can happen behind the scenes when an application connector or a GUI tool opens a new database session. 

Drivers and client tools commonly run series of statements to set up session configuration (e.g, SET SESSION variable = value). 

This increases the cost of creating new connections and delays when your application can start issuing queries.

The cost of connection handshakes becomes even more important if your applications are very sensitive to latency. 

OLTP or key-value workloads that expect single-digit millisecond latency can be visibly impacted if each connection is expensive to open. 

For example, if the driver runs six statements to set up a connection and each statement takes just one millisecond to run, your application will be delayed by six milliseconds before it issues its first query.


### Load balancing with the reader endpoint

Because the reader endpoint contains all Aurora Replicas, it can provide DNS-based, round robin load balancing for new connections. 
Every time you resolve the reader endpoint, you'll get an instance IP that you can connect to, chosen in round robin fashion.

DNS load balancing works at the connection level (not the individual query level). 
You must keep resolving the endpoint without caching DNS to get a different instance IP on each resolution. 


### Designing for fault tolerance and quick recovery

In large-scale database operations, you’re statistically more likely to experience issues such as connection interruptions or hardware failures. 
You must also take operational actions more frequently, such as scaling, adding, or removing DB instances and performing software upgrades.
The only scalable way of addressing this challenge is to assume that issues and changes will occur and design your applications accordingly.



#### Server configuration

There are two major server configuration variables worth mentioning in the context of this whitepaper: max_connections and max_connect_errors.

###### Configuration variable max_connections

The configuration variable max_connections limits the number of database connections per Aurora DB instance. 

The best practice is to set it slightly higher than the maximum number of connections you expect to open on each instance.

If you also enabled performance_schema, be extra careful with the setting. 

The Performance Schema memory structures are sized automatically based on server configuration variables, including max_connections. 

The higher you set the variable, the more memory Performance Schema uses. 

In extreme cases, this can lead to out-ofmemory issues on smaller instance types.


##### Configuration variable max_connect_errors

The configuration variable max_connect_errors determines how many successive interrupted connection requests are permitted from a given client host. 

If the client host exceeds the number of successive failed connection attempts, the server blocks it.


#### Conclusion

Understanding and implementing connection management best practices is critical to achieve scalability, reduce downtime, and ensure smooth integration between the
application and database layers. You can apply most of the recommendations provided in this whitepaper with little to no engineering effort. The guidance provided in this whitepaper should help you introduce improvements in your current and future application deployments using Aurora MySQL DB clusters.


### Reference 

<a href="https://d1.awsstatic.com/whitepapers/RDS/amazon-aurora-connection-management-handbook.pdf?did=wp_card&trk=wp_card"> Original paper </a>



