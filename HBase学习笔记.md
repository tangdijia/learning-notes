# HBase

## HBase-2.1.8 伪分布式部署

### 第一步

```shell
hbase-2.1.8-bin.tar.gz
tar -zxvf hbase-2.1.8-bin.tar.gz
```

### 第二步

设置环境变量

```shell
export HBASE_HOME=/opt/hbase-2.1.8
export PATH=$PATH:$HBASE_HOME/bin
```

### 第三步

```shell
vim conf/hbase-env.sh
```

```shell
export JAVA_HOME=/opt/jdk1.8.0_212
export HBASE_CLASSPATH=/opt/hbase-2.1.8/conf
# 默认是true，true的意思是使用hbase自带的zookeeper，false则是使用外部zookeeper(推荐)
export HBASE_MANAGES_ZK=true
```

### 第四步

```shell
vim conf/hbase-site.xml
```

```xml
<configuration>
  <!--
  指定hbase中的数据存放的本地目录，可以设置为hdfs上的路径，但搭建伪分布式，没有必要
  -->
  <property>
    <name>hbase.rootdir</name>
    <value>hdfs://devhost47:8020/user/hbase</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/opt/hbase-2.1.8/data/zookeeper</value>
  </property>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <!--
	HBase的运行模式，false是单机模式，true是分布式模式，若为false,HBase和Zookeeper会运行在同一个JVM里面
	默认: false
  -->
  <property>
    <name>hbase.unsafe.stream.capability.enforce</name>
    <value>false</value>
  </property>
  <!--
  HBase Master web 界面端口 设置为-1，意味着你不想让他运行
  0.98 版本以后默认: 16010，以前是60010
  -->
  <property>
    <name>hbase.master.info.port</name>
    <value>16010</value>
  </property>
  <!--
	HBase RegionServer web 界面绑定的端口 设置为 -1 意味这你不想与运行 RegionServer 界面.
	0.98 以前默认: 60030 以后默认是：16030
  -->
  <property>
    <name>hbase.regionserver.info.port</name>
    <value>16030</value>
  </property>
</configuration>
```

### 验证

HBase Master web: http://ip:16010/

HBase RegionServer web: http://ip:16030/

```shell
jps
```

```shell
42737 HMaster
42893 HRegionServer
```