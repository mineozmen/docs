# Typical Use Cases

State managers can be configured and used for a variety of scenarios, such as:

## **Basic Data Storage**

A single state manager managing records such as products, as the master data source.

This is the most standard use case, where all microservices use the same state manager for writing and reading records from a single database table.

## **Read/Write Replication**

Having separated read and write state managers, typically hosted on different servers, with real-time synchronization using built-in Rierino CDC capabilities.

This scenario is typically used for implementing CQRS pattern, separating high volume of reads from infrequent writes, while allowing optimization of read data store as per query requirements.

## **Search Engine**

Having a dedicated search engine, such as Elasticsearch, which is also managed as a state manager, with real-time synchronization from another state manager, again using built-in Rierino CDC capabilities. A search engine typically has a state manager for updating its records and a query manager for executing actual queries on it.

This scenario is typically used for supporting advanced and high performance querying requirements.

## **Cache Storage**

Having a dedicated cache store, such as Redis or even in-memory map structures, which is used for decreasing response latency in data read operations. Rierino supports both shared and local cache storage alternatives via different state manager types.

This scenario is typically useful for low-latency use cases, or when the number of records is relatively low and can be easily managed in memory.

## **Configuration Storage**

Using a state manager for storing Rierino configuration, such as saga definitions, runner elements.

This scenario is inherintly used by Rierino, where all configuration details are stored in a database. Runners, API gateways and certain handlers utilize these state managers to load their respective settings.
