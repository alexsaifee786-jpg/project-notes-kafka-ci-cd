# 02 · Kafka Cluster, Topics & Partitions

## 🟦 One definition per concept

| Concept | Meaning |
|---|---|
| Broker | Kafka server storing partition replicas and serving clients |
| Topic | Named stream of records, divided into partitions |
| Partition | Ordered record log enabling parallel processing |
| Offset | Record position within one partition; not a global event ID |
| Replication factor | Total copies of each partition, including the leader |
| ISR | In-sync replicas Kafka considers caught up with the leader |

## 🟩 Partitions distribute data; replicas copy it

**Illustration only:** three partitions with replication factor three. Business-topic settings and current leaders require runtime inspection.

| Partition | Broker 1 | Broker 2 | Broker 3 |
|---|---|---|---|
| P0 | Leader | Follower | Follower |
| P1 | Follower | Leader | Follower |
| P2 | Follower | Follower | Leader |

Three partitions × RF three = nine replicas, not nine different streams. Producers write to partition leaders; followers replicate.

- **Ordering:** within a partition, not globally across a topic.
- **Key:** with key-based partitioning and an unchanged partition count, the same key routes to the same partition.
- **Parallelism:** in normal group assignment, consumers beyond the partition count are idle.

## 🟪 KRaft and the actual network

KRaft manages metadata through a controller quorum without ZooKeeper. This local Kafka 4.3.1 setup has three combined broker/controller nodes. The three-controller quorum needs two for a majority.

| Broker | Windows host | Inside Docker |
|---|---|---|
| 1 | localhost:9092 | kop-kafka-1:19092 |
| 2 | localhost:9094 | kop-kafka-2:19092 |
| 3 | localhost:9096 | kop-kafka-3:19092 |

Controller traffic uses 19093. All nodes share one laptop, so this does not protect against laptop failure.

- **listeners:** where a broker accepts connections.
- **advertised.listeners:** addresses returned for client connections.
- Bootstrap servers provide discovery; advertised addresses must also be reachable.
- Inside Docker, localhost means that container itself.

## 🟧 Producer durability

- **acks=all:** waits for all current ISR replicas to acknowledge.
- **min.insync.replicas:** minimum ISR size required for an acks=all write.
- **Example:** RF=3 and min ISR=2 permit writes with two in-sync replicas. With only one, writes are rejected. Leader loss needs an eligible replacement.
- **Verified:** Order producer uses acks=all. Compose sets internal offsets/transaction-topic RF=3 and transaction-topic min ISR=2.
- Those internal-topic settings do not establish business-topic RF or min ISR. Inspect them at runtime.
- Combined broker/controller roles suit this local setup; critical deployments should separate the roles. Consumer ACK is a separate concept (Chapter 04).

**Sources:** [Compose](https://github.com/alexsaifee786-jpg/kafka-order-platform/blob/main/docker-compose.yml), [Kafka KRaft](https://kafka.apache.org/42/operations/kraft/), [Kafka topic configuration](https://kafka.apache.org/43/generated/topic_config.html).
