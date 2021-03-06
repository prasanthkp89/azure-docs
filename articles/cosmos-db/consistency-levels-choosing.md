---
title: Choosing the right consistency level for your application that uses Azure Cosmos DB | Microsoft Docs
description: Choosing the right consistency level for your application in Azure Cosmos DB.
keywords: consistency, performance, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown

ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown

---

# Choose the right consistency level for your application

Distributed databases relying on replication for high availability, low latency or both, make the fundamental tradeoff between the read consistency vs. availability, latency, and throughput. Most commercially available distributed databases ask developers to choose between the two extreme consistency models: strong consistency and eventual consistency. Azure Cosmos DB allows developers to choose among the five well-defined consistency models: strong, bounded staleness, session, consistent prefix, and eventual. Each of these consistency models is well-defined, intuitive and can be used for specific real-world scenarios. Each of the five consistency models provide [availability and performance tradeoffs](consistency-levels-tradeoffs.md) and are backed by comprehensive SLAs. The following simple considerations will help you make the right choice in many common scenarios.

## SQL API and Table API

Consider the following points if your application is built by using Cosmos DB SQL API or Table API

- For many real-world scenarios, session consistency is optimal and it's the recommended option. For more information, see, [How-to manage session token for your application](how-to-manage-consistency.md#utilize-session-tokens).

- If your application requires strong consistency, it is recommended that you use bounded staleness consistency level.

- If you need stricter consistency guarantees than the ones provided by session consistency and single-digit-millisecond latency for writes, it is recommended that you use bounded staleness consistency level.  

- If your application requires eventual consistency, it is recommended that you use consistent prefix consistency level.

- If you need less strict consistency guarantees than the ones provided by session consistency, it is recommended that you use consistent prefix consistency level.

- If you need the highest availability and lowest latency, then use eventual consistency level.

## Cassandra, MongoDB, and Gremlin API

- For details on mapping between “Read Consistency Level” offered in Apache Cassandra and Cosmos DB consistency levels, see [Consistency levels and Cosmos DB APIs](consistency-levels-across-apis.md#cassandra-mapping).

- For details on mapping between “Read Concern” of MongoDB and Azure Cosmos DB consistency levels, see [Consistency levels and Cosmos DB APIs](consistency-levels-across-apis.md#mongo-mapping).

## Consistency guarantees in practice

You may get stronger consistency guarantees in practice. Consistency guarantees for a read operation correspond to the freshness and ordering of the database state that you request. Read-consistency is tied to the ordering and propagation of the write/update operations.  

* When the consistency level is set to **bounded staleness**, Cosmos DB guarantees that the clients always read the value of a previous write, with a lag bounded by the staleness window.

* When the consistency level is set to **strong**, the staleness window is equivalent to zero, and the clients are guaranteed to read the latest committed value of the write operation.

* For the remaining three consistency levels, the staleness window is largely dependent on your workload. For example, if there are no write operations on the database, a read operation with **eventual**, **session**, or **consistent prefix** consistency levels is likely to yield the same results as a read operation with strong consistency level.

If your Cosmos DB account is configured with a consistency level other than the strong consistency, you can find out the probability that your clients may get strong and consistent reads for your workloads by looking at the Probabilistic Bounded Staleness (PBS) metric. This metric is exposed in the Azure portal, to learn more, see [how to use the PBS metric](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric) article.

Probabilistic bounded staleness shows how eventual is your eventual consistency. This metric provides an insight into how often you can get a stronger consistency than the consistency level that you have currently configured on your Cosmos DB account. In other words, you can see the probability (measured in milliseconds) of getting strongly consistent reads for a combination of write and read regions.

## Next steps

Read more about the consistency levels in the following articles:

* [Consistency level mapping across Cosmos DB APIs](consistency-levels-across-apis.md)
* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md)
* [How to manage the session token for your application](how-to-manage-consistency.md#utilize-session-tokens)
* [How to monitor the Probabilistically Bounded Staleness (PBS) metric](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)