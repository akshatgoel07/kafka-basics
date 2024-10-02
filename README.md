# Running Kafka in Docker

```bash
# Start Kafka container
docker run -p 9092:9092 apache/kafka:3.7.1

# Check running containers
docker ps

# Access Kafka container
docker exec -it container_id /bin/bash

# Navigate to Kafka bin directory
cd /opt/kafka/bin

# Create a topic
./kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092

# Publish to the topic
./kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092

# Consume from the topic
./kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092

```

### What is Apache Kafka?
Apache Kafka is a distributed streaming platform that allows for publishing and subscribing to streams of records, storing streams of records in a fault-tolerant way, and processing streams of records as they occur.

### What are the core APIs in Kafka?
Kafka has four core APIs:
1. Producer API
2. Consumer API
3. Streams API
4. Connector API

### What is a topic in Kafka?
A topic is a category or feed name to which records are published. Topics in Kafka are always multi-subscriber; that is, a topic can have zero, one, or many consumers that subscribe to the data written to it.

### What is a partition in Kafka?
A partition is an ordered, immutable sequence of records that is continually appended to. The records in the partitions are each assigned a sequential id number called the offset.

### What is a broker in Kafka?
A Kafka broker is a server that runs in a Kafka cluster. Multiple brokers work together to form the Kafka cluster and handle the storage and transmission of messages.

### What is the role of ZooKeeper in Kafka?
ZooKeeper is used for managing and coordinating Kafka brokers. It maintains metadata information about the Kafka cluster, such as the list of brokers, topics, partitions, and their leaders.

### Explain the concept of consumer groups in Kafka.
Consumer groups allow a group of instances to cooperate to consume a set of topics. Each consumer in a group reads from a unique subset of partitions in each topic they subscribe to, ensuring scalability and fault tolerance.

## Intermediate Concepts

### How does Kafka handle message retention?
Kafka retains messages for a configurable amount of time or until a size threshold is met. This is controlled by the retention policy, which can be set on a per-topic basis.

### Explain the concept of replication in Kafka.
Replication in Kafka involves creating copies of partitions across multiple brokers. This ensures fault tolerance and high availability. Each partition has one leader and zero or more followers.

### What is the ISR (In-Sync Replicas) in Kafka?
ISR stands for In-Sync Replicas. It is the set of replica brokers that are up-to-date with the leader for a given partition. The ISR includes the leader and all followers that are currently in sync with the leader.

### How does Kafka achieve high throughput?
Kafka achieves high throughput through:
1. Sequential I/O operations
2. Zero-copy principle for efficient data transfer
3. Batching of messages
4. Partitioning for parallel processing
5. Compression of messages

### Explain the difference between Kafka and traditional message queue systems.
Key differences include:
1. Persistence: Kafka stores messages on disk, allowing for replay
2. Scalability: Kafka is designed for distributed systems and horizontal scaling
3. Stream processing: Kafka supports real-time stream processing
4. Multi-subscriber model: Multiple consumers can read from the same topic

### What are the guarantees provided by Kafka in terms of message delivery?
Kafka provides the following guarantees:
1. Messages sent by a producer to a particular topic partition will be appended in the order they are sent
2. A consumer instance sees records in the order they are stored in the log
3. For a topic with replication factor N, Kafka will tolerate up to N-1 server failures without losing any records committed to the log

### How would you configure a Kafka producer for guaranteed message delivery?
To configure a Kafka producer for guaranteed message delivery:
1. Set `acks=all` to ensure all replicas receive the message
2. Use `retries` with a high value
3. Implement an idempotent producer by setting `enable.idempotence=true`
4. Use `max.in.flight.requests.per.connection=1` to ensure strict ordering

### How can you improve consumer performance in Kafka?
To improve consumer performance:
1. Increase the number of partitions to allow more consumers in a group
2. Adjust `fetch.min.bytes` and `fetch.max.wait.ms` for optimal batching
3. Increase `max.poll.records` to process more records per poll
4. Use appropriate deserialization methods
5. Implement parallel processing within the consumer

### How would you handle message ordering in Kafka when using multiple partitions?
To handle message ordering with multiple partitions:
1. Use a single partition if strict global ordering is required
2. Use a partitioning key for related messages to ensure they go to the same partition
3. Implement application-level sequencing or timestamping if partial ordering is acceptable

### Explain the concept of exactly-once semantics in Kafka and how to achieve it.
Exactly-once semantics ensure that each message is processed once and only once, even in the case of producer or consumer failures. To achieve this:
1. Use an idempotent producer (`enable.idempotence=true`)
2. Implement transactional producers and consumers
3. Configure consumers with `isolation.level=read_committed`
4. Use the transactional API for atomic multi-partition writes

### What is the purpose of compacted topics in Kafka and when would you use them?
Compacted topics in Kafka maintain the latest value for each key in the log. They are useful for:
1. Maintaining a changelog of data
2. Building materialized views or caches
3. Implementing event sourcing patterns
Compacted topics are ideal when you're interested in the final state of data rather than intermediate updates.

### Describe the Kafka Connect framework and its use cases.
Kafka Connect is a framework for scalably and reliably streaming data between Apache Kafka and other systems. It makes it simple to quickly define connectors that move large collections of data into and out of Kafka. Use cases include:
1. Streaming database changes to Kafka
2. Collecting metrics and logs into Kafka
3. Streaming Kafka data to storage and analytics systems

### Explain the Kafka Streams API and its key concepts.
Kafka Streams is a client library for building applications and microservices, where the input and output data are stored in Kafka clusters. Key concepts include:
1. StreamsBuilder: Used to construct topology of the streaming application
2. KStream: Represents a stream of records
3. KTable: Represents a changelog stream
4. GlobalKTable: Represents a fully replicated table
5. State Stores: Used for stateful operations
