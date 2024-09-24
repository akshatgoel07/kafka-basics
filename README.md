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
