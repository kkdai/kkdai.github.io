---
layout: post
title: "[TIL][SMACK] Install and run Kafka in Mac OSX"
description: ""
category: 
- TodayILearn
- Kubernetes
tags: ["TIL", "kafka", "docker"]

---

## Why not Kafka 0.11 or 1.0

- homebrew kafka version using 0.11 and could not launch on my computer, and it is hard to know detail why homebrew/kafka failed. ([issue](https://github.com/Homebrew/homebrew-services/issues/92))
- Transactional Coordinator still not support by sarama golang client ([golang](Shopify/sarama )) ([issue](https://github.com/Shopify/sarama/issues/901))


## Install Kafka 0.8 manually in 2017/11

```
sudo su - 
cd /tmp 
wget https://archive.apache.org/dist/kafka/0.8.2.2/kafka_2.9.1-0.8.2.2.tgz
tar -zxvf kafka_2.9.1-0.8.2.2.tgz -C /usr/local/
cd /usr/local/kafka_2.9.1-0.8.2.2

sbt update
sbt package

cd /usr/local
ln -s kafka_2.9.1-0.8.2.2 kafka

echo "" >> ~/.bash_profile
echo "" >> ~/.bash_profile
echo "# KAFKA" >> ~/.bash_profile
echo "export KAFKA_HOME=/usr/local/kafka" >> ~/.bash_profile
source ~/.bash_profile

echo "export KAFKA=$KAFKA_HOME/bin" >> ~/.bash_profile
echo "export KAFKA_CONFIG=$KAFKA_HOME/config" >> ~/.bash_profile
source ~/.bash_profile

$KAFKA/zookeeper-server-start.sh $KAFKA_CONFIG/zookeeper.properties
$KAFKA/kafka-server-start.sh $KAFKA_CONFIG/server.properties
```

## How to verify your installation?

(in your kafka path)


#### Create topic

```
> $KAFKA/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test`
```

#### Verify it

```
> $KAFKA/in/kafka-topics.sh --list --zookeeper localhost:2181

test
```

#### Or your can use golang client to verify it:

- [Console consumer](https://github.com/Shopify/sarama/tree/master/tools/kafka-console-consumer) 
- [Console producer](https://github.com/Shopify/sarama/tree/master/tools/kafka-console-producer)


## Troubleshooting 

- Check if any port already occupy by some app. refer: ([Who is listening on a given TCP port on Mac OS X?](https://stackoverflow.com/questions/4421633/who-is-listening-on-a-given-tcp-port-on-mac-os-x))
- After your uninstall zookeeper, the zookeeper service will not be removed. Please remember to remove it manually.

## Reference:

- [Kafka 0.8 on Mac OSX installation](https://gist.github.com/ekampf/8349429) 
- [Who is listening on a given TCP port on Mac OS X?](https://stackoverflow.com/questions/4421633/who-is-listening-on-a-given-tcp-port-on-mac-os-x)
- [Kafka Quickstart](https://kafka.apache.org/quickstart)
- [Mac brew kafka](http://www.jianshu.com/p/2b209d74139c)