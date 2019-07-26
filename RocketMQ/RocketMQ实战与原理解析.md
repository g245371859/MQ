# 一、快速入门

1. 下载`rocketmq-all-4.5.1-bin-release.zip`

2. `unzip`

3. 目录简单介绍

    ```shell
    benchmark	#运行benchmark程序的shell脚本
    bin			#各种使用RocketMQ的shell脚本(Linux平台)和cmd脚本(Windows平台)
    conf		#配置
    lib 		#依赖目录
    LICENSE 	#这三个是版权和功能性的说明
    NOTICE
    README.md
    ```

4. 启动

    ```shell
    nohup sh bin/mqnamesrv &
    nohup sh bin/mqbroker -n localhost:9876 &
    #注意，rocketmq在开启时需要很大的内存，需要修改bin/runbroker.sh里的jvm参数
    ```

5. 终止

    ```shell
    sh bin/mqshutdown namesrv
    sh bin/mqshutdown broker
    ```


# 二、生产环境下的配置和使用

## 1. 相关概念

- Producer 		生产者
- Consumer      消费者
- Broker             暂存处 
- NameServer   消息分发

> 启动 RocketMQ 的顺序是先启动 NameServer，再启动 Broker，这时候消息队列已经可以提供服务了，想发送消息就使用 Producer 来发送，想接收消息就使用 Consumer 来接收。很多应用程序既要发送，又要接收，可以启动多个Producer 和 Consumer 来发送多种消息，同时接收多种消息 。
>
> 为了消除单点故障，增加可靠性或增大吞吐量 ，可以在多台机器上部署多个 NameServer 和 Broker，为每个 Broker 部署一个或多个 Slave 。
>
> ![](/Users/apple/Downloads/ghf/mine/MQ/RocketMQ/pic/1564120802390.jpg)

- Topic 和 MessageQueue

> 一个分布式消息队列中间件部署好以后，可以给很多个业务提供服务，同一个业务也有不同类型的消息要投递 ， 这些不同类型的消息以不同的 Topic 名称来区分 。 所以发送和接收消息前，先创建 Topic ， 针对某个 Topic 发送和接收消息 。 有了 Topic 以后，还需要解决性能问题 。 如果一个 Topic 要发送和接收的数据量非常大， 需要能支持增加并行处理的机器来提高处理速度，这时候一个 Topic 可以根据需求设置一个或多个 Message Queue, Message Queue 类似分区或 Partition 。 Topic 有 了 多个 Message Queue 后，消息可以并行地向各个Message Queue 发送，消费者也可以并行地从多个 Message Queue 读取消息并消费 。