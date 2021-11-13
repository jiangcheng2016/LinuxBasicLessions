# Filebeat整合kafka

###### 1.1 ```input(log) + output(kafka)```

1.  修改```filebeat.yml```文件

```shell
# 先复制一份 filebeat.yml
cp filebeat.yml filebeat-log-kafka.yml

# 修改filebeat-log-kafka.yml

#增加kafka的配置
output.kafka:
    # initial borkers for reading cluster metadata
    hosts: ["192.168.201.32:9092","",""]  #注意如果是单机就写一个即可
    # message topic selection + partitioning
    topic: 'xxxx'  #topic需要提前创建
    partition.round_robin:
      reachable_only: false
    
    required_acks: 1
    compression: gzip
    max_message_bytes: 1000000
    
```

2. 启动```filebeat```

```shell
# 使用上面已经修改的 filebeat-log-kafka.yml 文件启动 filebeat
./filebeat -c filebeat-log-kafka.yml

# 启动后使用发现输出信息过多
# 可以通过配置限制output.kafka的数据输出
output.kafka:
    # initial borkers for reading cluster metadata
    hosts: ["192.168.201.32:9092","",""]  #注意如果是单机就写一个即可
    # message topic selection + partitioning
    topic: 'xxxx'  #topic需要提前创建
    partition.round_robin:
      reachable_only: false
    
    required_acks: 1
    compression: gzip
    max_message_bytes: 1000000
    codec.format:
      string: '%{[message]}'  #通过string格式来输出解析的message字段
```



