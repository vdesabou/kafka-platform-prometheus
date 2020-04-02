# kafka-platform-prometheus

Simple demo of how to monitor Kafka Platform using Prometheus and Grafana.

# Prerequisites

You need to have docker and docker-compose installed.

# Getting started

```bash
git clone https://github.com/jeanlouisboudart/kafka-platform-prometheus.git
cd kafka-platform-prometheus
```

# Build local images
This repository contains some local docker images including :
* jmx_exporter
* a simple producer
* as simple consumer

To build all images you just need to run :

```bash
docker-compose build
```

# Start the environment
To start the environment simply run the following command
```bash
docker-compose up -d
```

Open a brower and visit http://localhost:3000 (grafana).
Login/password is admin/admin.

# Destroy the environment
To destroy the environment simply run the following command to destroy containers and associated volumes :
```bash
docker-compose down -v
```

# Advanced

## Create a topic

Create `demo-perf-topic` with 4 partitions and 3 replicas.

```bash
docker-compose exec kafka-1 bash -c 'KAFKA_OPTS="" kafka-topics --create --partitions 4 --replication-factor 3 --topic demo-perf-topic --zookeeper zookeeper-1:2181'
```

## Produces random messages

Open a new terminal window and generate random messages to simulate producer load.

```bash
docker-compose exec kafka-1 bash -c 'KAFKA_OPTS="" kafka-producer-perf-test --throughput 500 --num-records 100000000 --topic demo-perf-topic --record-size 100 --producer-props bootstrap.servers=localhost:9092'
```

## Consumes random messages

Open a new terminal window and generate random messages to simulate consumer load.

```bash
docker-compose exec kafka-1 bash -c 'KAFKA_OPTS="" kafka-consumer-perf-test --messages 100000000 --threads 1 --topic demo-perf-topic --broker-list localhost:9092 --timeout 60000'
```
## Setup

<div style="display: flex; justify-content: center;">
 <img src="https://github.com/ksilin/kafka-platform-prometheus/blob/master/images/monitoring.setup.svg" height="500">
</div>

## Dashboards


### Kafka Dashboard

![Kafka 1](./images/kafka1.jpg)

![Throughput](./images/kafka2.jpg)


### Producer Dashboard

![System](./images/producer1.jpg)

![Throughput](./images/producer2.jpg)

![Performance](./images/producer3.jpg)

![Produce Request Metrics](./images/producer4.jpg)

![Connections](./images/producer5.jpg)

![Errors & Retries and Misc](./images/producer6.jpg)


### Consumer Dashboard

![Consumer](./images/consumer1.jpg)

### Consumer Lag Dashboard

This is using [kafka-lag-exporter](https://github.com/lightbend/kafka-lag-exporter) in order to pull consumer lags metrics from kafka cluster and be exported to Prometheus.

![Consumer Lag 1](./images/consumerlag1.jpg)

![Consumer Lag 2](./images/consumerlag2.jpg)
