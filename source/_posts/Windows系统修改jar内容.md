---
title: Windows系统修改jar包中的文件或者依赖
date: 2022-07-29 09:20:52
tags:
  - jar包

categories: [操作系统, Windows]
---
## 场景

应用的jar包在安全扫描时，发现不可信依赖，springboot依赖的tomcat几个包版本需要升级，现场环境是Linux，怎么进行依赖jar的替换，一搜一大把，就不赘述了。
可以参考如下链接：

```url
    https://blog.csdn.net/Mr_7777777/article/details/125152748
```

本文主要说一下Windows系统，如何达到上述目的。

### 操作步骤

下面是具体的操作步骤

#### 打开windows命令窗口，查看jar命令是否可用

![avatar](https://pic.imgdb.cn/item/62e33bcaf54cd3f937109bf3.png)
如果提示，`jar`不是内部或外部命令，也不是可运行的程序，则需要进行环境便的配置

##### jar环境变量配置

可以参考如下链接：

```url
    https://blog.csdn.net/sinat_20593627/article/details/109613547
```

如果是java1.8版本，会提示`bin\jlink.exe`不是内部或外部命令，也不是可运行的程序，这个忽略即可。

#### 解压jar包并手动替换，然后再重新打包

- 解压原jar包

  ```text
  jar -xvf xxx.jar
  ```

- 手动替换文件或者依赖
- 重新打包
  
  ```text
  jar -cfM0 test.jar BOOT-INF/ META-INF/ org/
  ```

以上就是所有内容，主要就是jar这个命令，其实是不区分操作系统，Linux和Windows系统的区别也就是删除和替换操作，解压和打包其实是一致的。
