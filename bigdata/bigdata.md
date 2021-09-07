## zookeeper

## kafka

### 简介

kafka是最初由Linkedin公司开发，是一个分布式、分区的、多副本的、多订阅者，基于zookeeper协调的分布式日志系统。

### 特性

- 高吞吐量、低延迟：kafka每秒可以处理几十万条消息，它的延迟最低只有几毫秒
- 可扩展性：kafka集群支持热扩展
- 持久性、可靠性：消息被持久化到本地磁盘，并且支持数据备份防止数据丢失
- 容错性：允许集群中节点失败
- 高并发：支持数千个客户端同时读写

### 使用场景

- 日志收集：收集各种服务的日志。
- 消息系统：解耦和生产者和消费者、缓存消息等。

### 部署

### 基本术语

### 使用

### API

### 调优

1. 设置`acks = all`。`acks`是`Producer`的一个参数，代表“已提交”消息的定义。如果设置成`all`，则表明所有`Broker`都要接收到消息，该消息才算是“已提交”。
2. 设置`retries`为一个较大的值。同样是`Producer`的参数。当出现网络抖动时，消息发送可能会失败，此时配置了`retries`的`Producer`能够自动重试发送消息，尽量避免消息丢失。
3. 设置`unclean.leader.election.enable = false`。控制哪些`Broker`有资格竞选分区的`Leader`。如果一个`Broker`落后原先的`Leader`太多，那么它一旦成为新的`Leader`，将会导致消息丢失。故一般都要将该参数设置成`false`。
4. 设置`replication.factor >= 3`。保存多份消息冗余。
5. 设置`min.insync.replicas > 1`。`Broker`端参数，控制消息至少要被写入到多少个副本才算是“已提交”。设置成大于 1 可以提升消息持久性。在生产环境中不要使用默认值 1。确保`replication.factor > min.insync.replicas`。如果两者相等，那么只要有一个副本离线，整个分区就无法正常工作了。推荐设置成`replication.factor = min.insync.replicas + 1`。
6. 确保消息消费完成再提交。`Consumer`端有个参数`enable.auto.commit`，最好设置成`false`，并自己来处理`offset`的提交更新。

### 问题

kafka如何保证消息不丢失?

1. 已提交的消息，即committed message。kafka只对已提交的消息做持久化保证。
2. kafka集群必须保证有足够broker正常工作，才能对消息做持久化做保证。

- 生产者端：
  - 设置acks=all， 当leader接受到消息，并同步到了一定数量的follower，才向生产者发生成功的消息，同步到的follower数量由 broker 端的min.insync.replicas 决定。
  - min.insync.replicas > 1消息至少要被写入到这么多副本才算成功。
  - 设置retries，出现网络抖动时，消息发送可能会失败，此时配置了retries的Producer能够自动重试发送消息，尽量避免消息丢失。

- 消费者端：
  - 如果在消息处理完成前就提交了offset，那么就有可能造成数据的丢失，由于kafka consumer默认是自动提交位移的。enable.auto.commit = false 关闭自动提交位移，在消息被完整处理之后再手动提交位移。

## hadoop

## hive

## hbase

## spark

## flink

## elasticsearch

## flume

## redis