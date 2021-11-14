# ```zookeeper```及```kafka```安装使用

#### 1.下载安装

##### 1.解压安装包

```shell
tar -zxvf zookeeper
```





#### 2.```kafka```安装使用

##### 2.1下载```kafka```安装包

链接

##### 2.2 解压安装包

```shell
tar -zxvf kafka_2.13-2.8.0.tgz

```

##### 2.3 修改配置文件

```shell
vim server.properties

#参数
broker id

log.dirs =/opt/kafka/data/kafka-logs #这里面存储的时kafka的核心数据

zookeeper.connect =localhost:2181 #zookeeper 地址及端口
```

##### 2.4 启动

```shell
# kafka默认是前台启动，加 -daemon 创建后台启动
# 在kafka目录下
bin/kafka-server-start.sh -daemon config/server.properties
```

![image-20211108170210774](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211108170210774.png)

```jps```查看进程，看到```Kafka```和```QuorumPeerMain```即为启动成功



##### 2.5 创建```topic```

```shell
# 副本数不能大于集群中broker的数量
# 如果是单机，partitions:分区，replication-factor:副本数只能为1
# 

bin/kafka-topics.sh --create --zookeeper localhost:2181 --partitions 1 --replication-factor 1 --topic helloTopic
 
 
 #查询topic 
 bin/kafka-topics.sh --list --zookeeper localhost:2181
 
 #topic详细信息
 bin/kafka-topics.sh --describe --zookeeper localhost:2181 helloTopic
 
```









```shell
bin/kafka-console-consumer.sh --bootstrap-server 172.19.2.198:9092 --topic test --from-beginning
  179  bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
  180  clear
  181  bin/kafka-console-consumer.sh --bootstrap-server 192.168.201.32:9092 --topic test --from-beginning
  182  ls
  183  cd /opt/kafka/
  184  ls
  185  cd kafka_2.13-2.8.0/
  186  ls
  187  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
  190  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
  191  clear
  192  bin/kafka-console-producer.sh --broker-list 192.168.201.32:9092 --topic test
  193  ls
  194  cd /
  195  ls
  196  cd opt/
  197  ls
  198  ll
  199  env='PATH=$PATH:$JAVA_HOME/bin'
  200  cat << EOF >> /etc/profile
  201  ]
  202  export JAVA_HOME=/opt/jdk1.8.0_301
  203  export $env
  204  EOF
  205  cat << EOF >> /etc/profileexport JAVA_HOME=/opt/jdk1.8.0_301
  206  export $env
  207  EOF
  208  source /etc/profile
  209  vim /etc/profile
  210  source /etc/profile
  211  java -version

  335  ls
  336  cd .

  353  ls
  354  cd kafka/
  355  ls
  356  cd kafka_2.13-2.8.0/
  357  ls
  358  bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
  359  bin/kafka-topics.sh --list --zookeeper localhost:2181


  392  bin/kafka-topics.sh --list --zookeeper localhost:2181
  393  ls
  394  cd ..


  485  ls
  486  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic streams-plaintext-input
  487  ls
  488  cd ..
  489  ls
  490  ./kafka_stop.sh 
  491  ./zookeeper_stop.sh 
  492  ls
  493  cd /
  494  ls
  495  cd opt/
  496  ls
  497  cd kafka/]
  498  ls
  499  cd kafka/
  500  ls
  501  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic streams-plaintext-input
  502  bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic streams-plaintext-output --value-deserializer org.apache.kafka.common.serialization.LongDeserializer --property print.key=true
  503  cd kafka_2.13-2.8.0/
  504  ls
  505  bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic streams-plaintext-output --value-deserializer org.apache.kafka.common.serialization.LongDeserializer --property print.key=true
  506  bin/kafka-topics.sh --list --zookeeper localhost:2181

  525  ls
  526  cd kafka/

  538   bin/kafka-topics.sh --list --zookeeper localhost:2181
  539  ls
  540  cd kafka_2.13-2.8.0/
  541  ls
  542   bin/kafka-topics.sh --list --zookeeper localhost:2181
  543  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic streams-plaintext-input
  544  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
  545  bin/kafka-console-producer.sh --broker-list 192.168.201.32:9092 --topic test
  546  bin/kafka-console-producer.sh --broker-list localh192.168.201.32--topic streams-plaintext-input
  547  bin/kafka-console-producer.sh --broker-list 192.168.201.32:9092 --topic streams-plaintext-input
  548  bin/kafka-console-producer.sh --broker-list 192.168.201.32:9092 --topic test
  549  ls
  550  cd ..
  551  ls
  552  ./kafka_stop.sh 
  553  ./zookeeper_stop.sh 
  554  ls
  555  cd /
  556  ls
  557  cd opt/
  558  ls
  559  cd kafka/
  560  ols
  561  ls
  562  cd kafka_2.13-2.8.0/
  563  ls
  564  bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 -- topic test --from-beginning
  565  bin/kafka-console-consumer.sh --bootstrap-server 192.168.201.32:9092 -- topic test --from-beginning
  566  bin/kafka-console-consumer.sh --bootstrap-server 192.168.201.32:9092 --topic test --from-beginning
  567  bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic streams-wordcount-output --value-deserializer org.apache.kafka.common.serialization.LongDeserializer --property print.key=true
  568  bin/kafka-console-consumer.sh --bootstrap-server 192.168.201.32:9092 --topic streams-wordcount-output --value-deserializer org.apache.kafka.common.serialization.LongDeserializer --property print.key=true
  569  bin/kafka-console-consumer.sh --bootstrap-server 192.168.201.32:9092 --topic test --from-beginning
  570  ls
  571  ifconfig
  572  jps
  573  shutdown now

```

