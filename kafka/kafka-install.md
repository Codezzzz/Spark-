# kafka install

## [kafka install](https://kafka.apache.org/downloads)

## 1. kafka start

1. ./bin/zookeeper-server-start.sh config/zookeeper.properties => 2181
2. ./bin/kafka-server-start.sh config/server.properties 
3. ./bin/kafka-console-producer.sh --broker-list localhost:9092
    - 프로듀서 , 쉽게 말하자면 보내는 사람 (writer)
4. ./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topicname --from-beginning
   -  컨슈머 , 받는사람 (listen)
5. 토픽 생성 
   - ./bin/kafka-topics.sh -create -zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic topicname


## 2. Spark quick start to kafak

- ./bin/spark-shell --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.4.0
 ```sh
 val df = spark.readStream.format("kafka").option("kafka.bootstrap.servers", "{ip}:9092").option("subscribe", "{topicname}").load()
 ``` 
