---
title: Windows安装Hive
date: 2021-12-07 20:56:41
tags:
  - Hive
  - 数据库
categories: [Windows, 数据库]
---

## 安装包版本

JDK1.8.0.232(java1.8就可以，小版本号不强求)
链接：<https://pan.baidu.com/s/1ZpMEtomkT1nvY_0qTbwcjg>
提取码：ei07
Hadoop2.7.7
链接：<https://pan.baidu.com/s/1oL61X921-4Urd3UCwbrKKQ>
提取码：9aln
Hive2.2.1
链接：<https://pan.baidu.com/s/1fPkeEJSImStlcCbYnCUy4g>
提取码：02hg
mysql-connector-java-5.1.46.jar
链接：<https://pan.baidu.com/s/13ICebpZljlb39w9sm4O-pA>
提取码：ae9a

## JDK安装

下载解压，安装到非默认路径

### JDK环境变量配置

配置JDK环境变量，依次点击我的电脑-属性-高级系统设置-环境变量-新建系统变量，如下图所示：

![avatar](https://pic.imgdb.cn/item/637c2acd16f2c2beb17674a4.png)

编辑系统变量`Path`，添加如下图所示两个值

![avatar](https://pic.imgdb.cn/item/637c2d0316f2c2beb178b563.png)

## Hadoop安装

下载解压即可

### Hadoop环境变量配置

参考JDK环境变量配置，如下图所示：
![avatar](https://pic.imgdb.cn/item/637c720b16f2c2beb1da0696.png)

编辑系统变量`Path`，添加如下图所示一个值

![avatar](https://pic.imgdb.cn/item/637c720716f2c2beb1da0039.png)

环境变量配置完成后打开`cmd`进行测试，输入`hadoop`，正常应如下所示：

![avatar](https://pic.imgdb.cn/item/637c79ef16f2c2beb1e77294.png)

### 修改配置文件

#### 新建目录

新建namenode和datanode目录，新建data目录，在下面新增dfs目录，再在下面新增namenode和datannode目录

![avatar](https://pic.imgdb.cn/item/637c7b6416f2c2beb1e9c21e.png)

#### 修改core-site.xml文件

文件目录`E:\tools\Hadoop\hadoop-2.7.7\etc\hadoop`，将下面的代码复制到core_site.xml，并保存

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

#### 修改hdfs-site.xml（目录和core_site一致），datanode和namenode改为自己的目录

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/E:/tools/Hadoop/hadoop-2.7.7/data/dfs/namenode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/E:/tools/Hadoop/hadoop-2.7.7/data/dfs/datanode</value>
    </property>
</configuration>
```

#### 修改mapred-site.xml.template文件（目录和core_site一致），修改完重命名为mapred-site.xml

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

#### 修改yarn-site.xml文件（目录和core_site一致）

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
</configuration>
```

#### 修改hadoop-env.cmd文件（目录和core_site一致）

其实就是设置java的目录

```shell
@rem set JAVA_HOME=%JAVA_HOME%
set JAVA_HOME=D:\tools\java\jdk1.8.0.232
```

### 格式化HDFS，打开Hadoop

至此，hadoop配置基本结束，接下来就需要测试hadoop是否安装成功

格式化HDFS，打开cmd，输入`hdfs namenode -format`，结果如下即为成功

![avatar](https://pic.imgdb.cn/item/6387084416f2c2beb1343ddd.png)

切换到`E:\tools\Hadoop\hadoop-2.7.7\sbin`目录，执行`start-all.cmd`，会打开4个窗口即为成功
然后，输入`jps`命令测试是否成功，如下图
![avatar](https://pic.imgdb.cn/item/6387091a16f2c2beb13568d2.png)

### 结果测试

在`E:\tools\Hadoop\hadoop-2.7.7\sbin`目录下创建新的文件，然后在`http://localhost:50070/explorer.html#/`查看结果

```text
    hadoop fs -mkdir /tmp
    hadoop fs -mkdir /user/
    hadoop fs -mkdir /user/hive/
    hadoop fs -mkdir /user/hive/warehouse
    hadoop fs -chmod g+w /tmp
    hadoop fs -chmod g+w /user/hive/warehouse
```

![avatar](https://pic.imgdb.cn/item/63870c1916f2c2beb1399e74.png)

## HVIE安装

### 环境变量配置

参考JDK环境变量配置，如下图所示：
![avatar](https://pic.imgdb.cn/item/63870d4716f2c2beb13ada5b.png)

编辑系统变量`Path`，添加如下图所示一个值

![avatar](https://pic.imgdb.cn/item/63870dd216f2c2beb13c2054.png)

环境变量配置完成后打开`cmd`进行测试，输入`hive`，正常应如下所示：

![avatar](https://pic.imgdb.cn/item/63870f9316f2c2beb13ffc9d.png)

### 文件配置

#### 目录更改

修改目录`E:\tools\hive\conf`中的4个默认的配置文件模板拷贝成新的文件名

```text
    hive-default.xml.template -----> hive-site.xml
    hive-env.sh.template -----> hive-env.sh
    hive-exec-log4j.properties.template -----> hive-exec-log4j2.properties
    hive-log4j.properties.template -----> hive-log4j2.properties
```

#### 创建新目录

创建以下几个空目录

```text
    E:\tools\hive\my_hive\operation_logs_dir
    E:\tools\hive\my_hive\querylog_dir
    E:\tools\hive\my_hive\resources_dir
    E:\tools\hive\my_hive\scratch_dir
```

如下图所示
![avatar](https://pic1.imgdb.cn/item/638d488ab1fccdcd36c28371.png)

#### mysql驱动配置

将mysql-connector-java-5.1.46-bin.jar复制到`E:\tools\hive\lib`目录下
如下图所示
![avatar](https://pic1.imgdb.cn/item/638d4904b1fccdcd36c3118b.png)

#### 修改hive-env.sh文件

新增以下内容，路径注意修改为自己的

```text
# Set HADOOP_HOME to point to a specific hadoop install directory
HADOOP_HOME=E:\tools\Hadoop\hadoop-2.7.7

# Hive Configuration Directory can be controlled by:
export HIVE_CONF_DIR=E:\tools\hive\conf

# Folder containing extra ibraries required for hive compilation/execution can be controlled by:
export HIVE_AUX_JARS_PATH=E:\tools\hive\lib
```

#### 修改hive-site.xml文件

文件内容比较多，可以直接用我的，然后替换路径和mysql的账密即可

链接：<https://pan.baidu.com/s/1PqtKV8Filn7DSwmqssSlTw>
提取码：12gz

按照下图所示，查找修改即可
![avatar](https://pic1.imgdb.cn/item/638d4ae8b1fccdcd36c53503.png)
![avatar](https://pic1.imgdb.cn/item/638d4af4b1fccdcd36c54064.png)

### 创建数据库

配置文件完成后，创建数据库，注意字符集和排序规则的设置属性
![avatar](https://pic1.imgdb.cn/item/638d4c1eb1fccdcd36c656e8.png)

### 启动hive，结果测试

#### 启动hadoop

打开windows命令窗口，切换目录到`E:\tools\Hadoop\hadoop-2.7.7\sbin`，输入命令`start-dfs.cmd`并回车，启动两个窗口服务即成功

#### 启动hive metastore

在目录`E:\tools\Hadoop\hadoop-2.7.7\sbin`的命令窗口输入`hive -service meatstore`，如果在hive数据库中出现如下所示众多表，则说明开启成功

![avatar](https://pic.imgdb.cn/item/638d5cfab1fccdcd36dc1f86.png)
![avatar](https://pic.imgdb.cn/item/638d5cfab1fccdcd36dc1f75.png)

上述方式启动结果如下
![avatar](https://pic.imgdb.cn/item/638d5da8b1fccdcd36dd2772.png)

此外，如果要操作hive，需要使用命令`hive Starting Hive Metastore Server`，进入hive操作系统
![avatar](https://pic.imgdb.cn/item/638d5e07b1fccdcd36dda9cc.png)

此时，可以直接执行HQL语句进行测试，例如执行`create table stu(id int, name string);`，然后去
<http://localhost:50070/explorer.html#/user/hive/warehouse>查看结果，
![avatar](https://pic.imgdb.cn/item/638d5eacb1fccdcd36de9167.png)

如上所示，则hive在windows的安装成功。
