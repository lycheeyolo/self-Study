# Kafka
## 1. 核心概念
      1. Producer: 消息的生产者；
      2. Consumer: 消息的消费者；
      3. Consumer Group: 消费者组，可以并行消费Topic中的partition的消息；
      4. Broker: 缓存代理，Kafka集群中的一台或多台服务器统称broker；
      5. Topic: Kafka处理资源的消息源的不同分类；
      6. Partition: Topic物理上额分组，一个Topic可以分为多个partition，每个partition是一个有序的队列。partition中的每条消息都会被分配一个有序的Id(offset);
      7. Message: 消息，是通信的基本单位，每个producer可以向一个topic发布多个消息；每个消息三个属性：offset, MessageSize, data
      8. Producers: 消息和数据的生产者，向Kakfa的一个topic发布消息的过程叫做 producers；
      9. Consumers: 消息和数据的消费者，订阅Topic并处理其发布的消息的消费过程叫做consumers；
## 2. 设计思想
      1. Consumer Group: 
        多个consumer可以组成一个组，每个消息只能被组中的一个consumer消费。如果一个消息需要被多个consumer消费，那么这些consumer必须在不同的组中；
      2. 消息状态：
        在Kafka中，消息的状态被保存到consumer中，broker不会关心哪个消息被消费了，或者被谁消费了。只记录一个offset值（指向partition中的下一个要被消费的消息位置），这意味着如果consumer处理不好的话，broker上的一个消息可能被consumer多次消费;
      3. 消息持久化：
        Kafka中会把消息持久化到本地文件系统中，并且保持加高的效率；
      4. 消息有效期： 
        Kafka可以配置消息可处于订阅状态的时长；
