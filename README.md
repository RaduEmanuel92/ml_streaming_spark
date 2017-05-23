# Streaming ML on Apache Spark

## Setup

It is expected that you have,

- Anaconda distribution installed, preferably with Python 2.7.

- PySpark in the `$PYTHONPATH`. To test this try `import pyspark` in a Python shell. My configuration looks as follow (in file `.bashrc` in the HOME folder)

```
export SPARK_HOME=$HOME/spark # ... your spark directory ...
export PATH=$PATH:$SPARK_HOME/bin # add spark-submit, spark-shell etc. to PATH

export PYTHONPATH=$SPARK_HOME/python:$PYTHONPATH
export PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.10.4-src.zip:$PYTHONPATH

export PYSPARK_DRIVER_PYTHON=$ANACONDA_HOME/bin/python
export PYSPARK_PYTHON=$ANACONDA_HOME/bin/python
```

- A terminal, we work with the command line. This is *de-facto* standard for Data Scientists.

### Kafka

Kafka is a distributed and fault-tolerant message queue. Kafka depends on Zookeeper for keeping its state. To keep things simple, we run a docker container with _both_ Kafka and Zookeeper. In practice, this is never done, as both services have different requirements. 

It is wise, however, to install (get [this](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.12-0.10.2.1.tgz)) Kafka separately as this provides the CLI tool.

Unzip it via `tar -xzf kafka_2.12-0.10.2.1.tgz` and set `KAFKA_HOME` properly. I extracted it to `~/Downloads/kafka_2.12-0.10.2.1` and write

```
export KAFKA_HOME="~/Downloads/kafka_2.12-0.10.2.1"
cd $KAFKA_HOME
```

_Starting Zookeeper_

```
bash bin/zookeeper-server-start.sh config/zookeeper.properties
```

_Starting Kafka Broker_

```
bash bin/kafka-server-start.sh config/server.properties
```
_Console Consumer_

Useful to test a topic's contents.
```
kafka-console-consumer.sh \
  --bootstrap-server localhost:9092 \
  --from-beginning or --offset <offset> \
  --topic test
```

_Producing messages in terminal_

_note that the test topics gets created automatically_
```
bin/kafka-console-producer.sh --zookeeper localhost:2181 --topic test
```

_Topics_
```
kafka-topics.sh \
  --zookeeper localhost:2181 \
  --create --topic test-topic \
  --partitions 2 \
  --replication-factor 1
  
kafka-topics.sh --zookeeper localhost:2181 --describe

kafka-topics.sh --zookeeper localhost:2181 --delete --topic test-topic
```



_Consumer Groups_
```
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
```
 
 ### Toywiese
 
 Contains the first day's projects.
 
```
 └── toywiese
    ├── autodatascientist
    │   ├── consumer.py
    │   ├── exercise.txt
    │   ├── producer.py
    │   └── requirements.txt
    ├── simple_consumer
    │   ├── Dockerfile
    │   ├── requirements.txt
    │   ├── run.sh
    │   └── simple_consumer.py
    ├── simple_producer
    │   ├── Dockerfile
    │   ├── requirements.txt
    │   ├── run.sh
    │   └── simple_producer.py
    └── simple_sparkstream
        ├── requirements.txt
        ├── run.sh
        └── simple_sparkstream.py
```