# 添加hadoop2.7.0
## 下载hadoop2.7.0
https://archive.apache.org/dist/hadoop/core/hadoop-2.7.0/hadoop-2.7.0.tar.gz
## 安装测试hadoop
```
docker run -p 50070:50070 -p 8088:8088 -it bigdatadockerhub/bigdata:jdk8 /bin/bash
[root@da7006203d7b /]#
docker cp hadoop-2.7.0.tar.gz da7006203d7b:/opt/modules/
[root@da7006203d7b /]# cd /opt/modules/
[root@da7006203d7b modules]# ll
total 205416
-rw-r--r-- 1 root  root  210343364 Jan  3 08:11 hadoop-2.7.0.tar.gz
drwxr-xr-x 8 10143 10143       273 Sep 16 17:14 jdk1.8.0_271

[root@01521045db7c modules]# tar -zxf hadoop-2.7.0.tar.gz -C ./
[root@da7006203d7b modules]# rm hadoop-2.7.0.tar.gz
[root@da7006203d7b modules]# ll
total 0
drwxr-xr-x 9 10021 10021 149 Apr 10  2015 hadoop-2.7.0
drwxr-xr-x 8 10143 10143 273 Sep 16 17:14 jdk1.8.0_271
```
### 配置hadoop单机版
```
[root@da7006203d7b modules]# cd hadoop-2.7.0/
[root@da7006203d7b hadoop-2.7.0]# rm -rf share/doc/
[root@da7006203d7b hadoop-2.7.0]# rm -rf etc/hadoop/*.cmd
[root@da7006203d7b hadoop-2.7.0]# cd etc/hadoop/
[root@da7006203d7b hadoop]# mkdir -p /opt/modules/hadoop-2.7.0/tmp
```
#### hadoop-env.sh
```
[root@da7006203d7b hadoop]# vi hadoop-env.sh
export JAVA_HOME=/opt/modules/jdk1.8.0_271
```
#### core-site.xml
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
        <description>配置NameNode的主机地址及其端口号</description>
    </property>
    <property>
        <name>hadoop.http.staticuser.user</name>
        <value>kfk</value>
        <description>HDFS Web UI 用户名</description>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/modules/hadoop-2.7.0/tmp</value>
        <description>其他临时目录的基础。（HDFS等相关文件的存储目录基础，很重要）</description>
    </property>
</configuration>
```
#### hdfs-site.xml
```
<configuration>
    <!-- 单主节点配置 -->
    <property>
        <name>dfs.replication</name>
        <value>1</value>
        <description>设置文件副本数</description>
    </property>
    <property>
        <name>dfs.permissions.enabled</name>
        <value>false</value>
        <description>查看日志是否审核权限</description>
    </property>
    <property>
        <name>dfs.http.address</name>
        <value>localhost:50070</value>
        <description>hdfs的webUI地址</description>
    </property>
</configuration>
```
#### yarn-site.xml
```
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
        <description>服务名称</description>
    </property>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>localhost</value>
        <description>The hostname of the RM.</description>
    </property>
    <property>
        <name>yarn.log-aggregation-enable</name>
        <value>true</value>
        <description>开启日志聚集</description>
    </property>
    <property>
        <name>yarn.log-aggregation.retain-seconds</name>
        <value>259200</value>
        <description>收集的日志的保留时间，以秒为单位，到时后被删除，保留3天后删除</description>
    </property>
    <property>
        <name>yarn.nodemanager.local-dirs</name>
        <value>/opt/modules/hadoop-2.7.0/tmp</value>
        <description>Diagnostics: File /opt/modules/hadoop-2.7.0/data/tmp/nm-local-dir/usercache/kfk/appcache/application_159/container_159 does not exist Failing this attempt. Failing the application.</description>
    </property>
</configuration>

```
#### mapred-site.xml
```
[root@da7006203d7b hadoop]# mv mapred-site.xml.template mapred-site.xml
[root@da7006203d7b hadoop]# vi mapred-site.xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
        <description>mapreduce 运行架构</description>
    </property>
    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>localhost:10020</value>
        <description>MapReduce JobHistory Server IPC host:port</description>
    </property>
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>localhost:19888</value>
        <description>MapReduce JobHistory Server Web UI host:port</description>
    </property>
</configuration>
```
### 运行测试
```
格式化namenode
[root@da7006203d7b hadoop]# /opt/modules/hadoop-2.7.0/bin/hdfs namenode -format
启动hdfs: ./sbin/start-dfs.sh
启动yarn: ./sbin/start-yarn.sh 
```
