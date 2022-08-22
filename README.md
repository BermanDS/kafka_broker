### Deployment kafka cluster in example of SparkStrteaming using

Define .env from .env.example with correct credentials
By default after building kafka - created topic as name from .env.
## Kafka on single node

```bash
node > docker-compose -f docker-compose.yml up --build -d
```

## Kafka cluster Docker Swarm

```bash
node_manager > docker swarm init
```
Join workers

```bash
worker_node1 > docker swarm join --token <swarm-token> node_manager:2377
worker_node2 > docker swarm join --token <swarm-token> node_manager:2377
```
Setup labels in nodes
```bash
node_manager > docker node update --label-add zookeeper=1 node_manager
node_manager > docker node update --label-add kafka=1 node_manager
node_manager > docker node update --label-add kafka=2 worker_node1
node_manager > docker node update --label-add kafka=3 worker_node2
```

Creating on all nodes directories for kafka logs and properties
```bash
_node > mkdir /var/kafka_logs
_node > chmod -R 777 /var/kafka_logs
_node > mkdir /var/kafka_configs
_node > chmod -R 777 /var/kafka_configs
```
Create a overlay network: kafka-net
```bash
node_manager > docker network create --driver overlay kafka-net
```

images from on https://developer.confluent.io

Deploy Kafka Cluster
```bash
node_manager > docker stack deploy --compose-file docker-compose-swarm.yml kafka_cluster
```
