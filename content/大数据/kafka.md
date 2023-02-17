[SASL/SCRAM+ACL实现动态创建用户及权限控制](https://blog.csdn.net/ashic/article/details/86661599)、[Kafka名词解释等](https://www.cnblogs.com/biehongli/p/8335538.html)

**一、sasl scram**

```
# 创建
kafka-configs.sh --zookeeper zk1:2181 --alter --add-config 'SCRAM-SHA-256=[password=redhat],SCRAM-SHA-512=[password=redhat]' --entity-type users --entity-name kk-cluster-user
kafka-configs.sh --zookeeper zk1:2181 --alter --add-config 'SCRAM-SHA-256=[password=redhat],SCRAM-SHA-512=[password=redhat]' --entity-type users --entity-name kk-client1
# 查看
kafka-configs.sh --zookeeper zk1:2181 --describe --entity-type users --entity-name kk-client1
# 删除
kafka-configs.sh --zookeeper zk1:2181 --alter --delete-config 'SCRAM-SHA-512' --entity-type users --entity-name kk-client1
```

**二、acl操作**

```
# 查看
kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --list
# 指定权限，目前每次只能指定一个
kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --add --allow-principal User:kk-client1 --operation Read --topic test --allow-host "192.168.2.*"
# 生产者权限
kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --add --allow-principal User:kk-client1 --producer --topic test --allow-host "192.168.2.*"
# 消费者权限
kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --add --allow-principal User:kk-client1 --consumer --topic test --group "*" --allow-host "192.168.2.*"
# 前缀式
kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --add --allow-principal User:kk-client1 --producer --resource-pattern-type prefixed --topic "els-"
# group会同样使用前缀，没办法使用*匹配所有，需要使用特定授权
kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --add --allow-principal User:kk-client1 --consumer --resource-pattern-type prefixed --topic "els-" --group "cli-"

kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --add --allow-principal User:kk-client1 --operation Read --topic "els-" --resource-pattern-type prefixed
kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --add --allow-principal User:kk-client1 --operation Describe --topic "els-" --resource-pattern-type prefixed
kafka-acls.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --add --allow-principal User:kk-client1 --operation Read --group "*"
```

**三、主题操作**

```
# 查看现有
kafka-topics.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --list
# 创建
kafka-topics.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --create --topic test --partitions 1 --replication-factor 1
# 查看特定
kafka-topics.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --describe --topic test
```

**四、消费者**

```
kafka-consumer-groups.sh --command-config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --list

kafka-console-consumer.sh --consumer.config /etc/kafka/client-sasl.properties --bootstrap-server 127.0.0.1:9092 --topic test --from-beginning
```

**五、生产、消费性能测试**

```
kafka-producer-perf-test.sh --producer.config /etc/kafka/client-sasl.properties --producer-props bootstrap.servers=127.0.0.1:9092 acks=all --topic test --throughput 1000000 --num-records 1000000 --record-size 1000
kafka-consumer-perf-test.sh --consumer.config /etc/kafka/client-sasl.properties --broker-list 127.0.0.1:9092 --topic test --messages 1000000
```



