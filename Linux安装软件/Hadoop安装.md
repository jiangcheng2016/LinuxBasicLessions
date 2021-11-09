# Hadoop安装

### 1.```SSH```免密登录

### 2.Hadoop伪集群安装

##### 2.1解压Hadoop安装包

```shell
tar -zxvf 安装包名字
```

##### 2.2 将解压的文件夹移动到自定义文件夹下

```shell
# 个人使用目录 /opt/hadoop
# 安装包所在目录 /opt/anzhuangbao
#在 /opt 下执行：

mv anzhuangbao/hadoop-3.3.0 hadoop/
```

##### 2.3修改```etc/hadoop```下相关配置问价

1.修改 ```hadoop-env.sh```

在文件末尾添加一些配置信息

```shell
export JAVA_HOME=
export HADOOP_LOG_DIR= 指定一个新的目录/opt/hadoop/hadoop_repo/logs/hadoop


```

2.修改```core-site.xml```文件

添加属性

```shell
#hadoop伪分布集群参数配置之core-site.xml

<configuration>
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://bigdata01:9000</value>
	</property>
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/data/hadoop_repo</value>
	</property>
</configuration>
```

3.修改 ```hdfs-site.xml```文件

添加属性

```shell
#hadoop伪分布集群参数配置之 hdfs-site.xml

<configuration>
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
</configuration>
```



4.修改 ```mapred-site.xml```文件

添加属性

```shell
#hadoop伪分布集群参数配置之 mapred-site.xml

<configuration>
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
</configuration>
```



5.修改 ```yarn-site.xml```文件

添加属性

```shell
#hadoop伪分布集群参数配置之 yarn-site.xml

<configuration>
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	<property>
		<name>yarn.nodemanager.env-whitelist</name>
		<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
	</property>
</configuration>
```

6.修改 ```workers```文件

这里面修改主机，从机的地址，里面原始是```localhost```，一台机器下可以不修改，也可以修改成当前的主机名

##### 2.4 格式化```HDFS```文件

启动之前需要对```hdfs```格式化，执行成功后，不要再次执行；如果格式化失败了，没有成功格式化，此时根据错误信息修改配置文件，然后再格式化

使用：

```shell
bin/hdfs namenode -format

#如果出现 has been successfully formatted
```

##### 2.5 启动

```shell
 # 启动命令
 sbin/start-all.sh
```

可能会出现缺失很多用户信息的错误，此时进行配置用户相关信息

==到 ```sbin```目录下：==

1. 修改```start-dfs.sh```文件
2. 修改```stop-dfs.sh```文件
3. 修改```start-yarn.sh```文件
4. 修改```stop-yarn.sh```文件

```shell
#在中间位置加配置信息

1.start-dfs.sh
HDFS_DATANODE_USER=root
HDFS_DATANODE_SECURE_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root

2.stop-dfs.sh
HDFS_DATANODE_USER=root
HDFS_DATANODE_SECURE_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root

3.start-yarn.sh
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root

4.stop-yarn.sh
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root

```















