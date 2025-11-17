# Kafka Command Reference – Most Frequently Used

## Most Frequently Used Commands

```bash
kafka-topics.sh --bootstrap-server localhost:9092 --list                       # List all topics in the cluster
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test --partitions 3 --replication-factor 1  # Create a new topic
kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic test      # Describe a topic (partitions, replicas, ISR)
kafka-console-producer.sh --broker-list localhost:9092 --topic test            # Start a simple producer on a topic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning  # Consume messages from the start
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list              # List all consumer groups
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-group  # Show lag and offsets for a group

kafka-broker-api-versions.sh --bootstrap-server localhost:9092                  # Show supported API versions by broker
kafka-cluster.sh --bootstrap-server localhost:9092 --describe                   # Describe cluster info (depends on Kafka tools version)
```