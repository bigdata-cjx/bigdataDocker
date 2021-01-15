# 添加Spark
## 下载spark-2.4.0
http://archive.apache.org/dist/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz
## 安装测试hadoop
```
8088:yarn wenui端口
50070:hadoop webui端口
50075:hadoop 下载文件端口
[root@bigdata-pro03 ~]# docker run -it -p 8088:8088 -p 50070:50070 -p 50075:50075 --net bigdata --ip 172.168.0.2 --name hadoop dockerhubbigdata/bigdata:jdk8 /bin/bash
root@11882bba6b10:~# 
docker cp hadoop-2.7.0.tar.gz 11882bba6b10:/opt/modules/
root@11882bba6b10:~# cd /opt/modules/
root@11882bba6b10:/opt/modules# tar -zxf hadoop-2.7.0.tar.gz -C ./
root@11882bba6b10:/opt/modules# rm hadoop-2.7.0.tar.gz
```
### 配置hadoop单机版
```
root@11882bba6b10:/opt/modules# cd hadoop-2.7.0/
root@11882bba6b10:/opt/modules/hadoop-2.7.0# rm -rf share/doc/
root@11882bba6b10:/opt/modules/hadoop-2.7.0# rm -rf etc/hadoop/*.cmd
root@11882bba6b10:/opt/modules/hadoop-2.7.0# cd etc/hadoop/
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# mkdir -p /opt/modules/hadoop-2.7.0/tmp
```
#### 导入配置文件（）
```
docker cp copy-files/ 11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop/
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# cp copy-files/* ./
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# rm -rf copy-files/
```
### 运行测试
```
service ssh start

root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# export JAVA_HOME=/opt/modules/jdk1.8.0_271
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# export PATH=$JAVA_HOME/bin:$PATH

格式化namenode
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# /opt/modules/hadoop-2.7.0/bin/hdfs namenode -format
启动hdfs: 
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# /opt/modules/hadoop-2.7.0/sbin/start-dfs.sh
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# jps
418 DataNode
279 NameNode
759 Jps
601 SecondaryNameNode
启动yarn:
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# /opt/modules/hadoop-2.7.0/sbin/start-yarn.sh
root@11882bba6b10:/opt/modules/hadoop-2.7.0/etc/hadoop# jps
418 DataNode
279 NameNode
823 ResourceManager
601 SecondaryNameNode
937 NodeManager
1257 Jps

root@11882bba6b10:/opt/modules/hadoop-2.7.0# /opt/modules/hadoop-2.7.0/bin/hdfs dfs -mkdir -p /user/root/data/
root@11882bba6b10:/opt/modules/hadoop-2.7.0# /opt/modules/hadoop-2.7.0/bin/hdfs dfs -put /opt/modules/hadoop-2.7.0/etc/hadoop/core-site.xml /user/root/data
root@11882bba6b10:/opt/modules/hadoop-2.7.0# /opt/modules/hadoop-2.7.0/bin/hdfs dfs -text /user/root/data/core-site.xml

WebUI查看：
```
### 提交镜像
```
docker commit hadoop dockerhubbigdata/bigdata:hadoop-alone
docker push dockerhubbigdata/bigdata:hadoop-alone
```
### 宿主机局域网测试
```
docker run -it -p 8088:8088 -p 50070:50070 -p 50075:50075 --net bigdata --ip 172.168.0.2 --name hadoop dockerhubbigdata/bigdata:hadoop-alone /bin/bash

service ssh start

root@150dc92d000c:/opt/modules/hadoop-2.7.0/etc/hadoop# export JAVA_HOME=/opt/modules/jdk1.8.0_271
root@150dc92d000c:/opt/modules/hadoop-2.7.0/etc/hadoop# export PATH=$JAVA_HOME/bin:$PATH

root@150dc92d000c:/opt/modules/hadoop-2.7.0/etc/hadoop# /opt/modules/hadoop-2.7.0/sbin/start-dfs.sh
root@150dc92d000c:/opt/modules/hadoop-2.7.0/etc/hadoop# /opt/modules/hadoop-2.7.0/sbin/start-yarn.sh
root@150dc92d000c:~# jps
787 SecondaryNameNode
599 DataNode
970 ResourceManager
459 NameNode
1420 Jps
1085 NodeManager
```
#### WebUI测试
```
http://192.168.3.153:50070/explorer.html#/user/root/data
下载时注意把容器id换成宿主机ip
http://bigdata-pro03:8088/cluster
```