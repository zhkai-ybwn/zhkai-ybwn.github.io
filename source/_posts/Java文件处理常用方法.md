---
title: Java文件处理常用方法
date: 2023-02-17 11:07:01
tags:
    - 文件处理
categories: [后端]
---

## Java文件处理常用方法

归纳整理一些常用的处理文件的方法

### Java Apache FileUtils

#### Maven依赖引入

```xml
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.6</version>
    </dependency>
```

#### 创建和删除文件

使用FileUtils.touch()创建一个新文件，并使用FileUtils.deleteQuietly()将其删除。
CreateDeleteFileEx.java

```java
import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;

/**
 * @author 25724
 * @date 2023/2/17 15:02
 * @description 创建和删除文件
 */
public class CreateDeleteFileEx {
    public static void main(String[] args) throws IOException {
        File myFile = new File("src/main/resources/myFile.txt");
        FileUtils.touch(myFile);
        if (myFile.exists()) {
            System.out.println("The file exists");
        } else {
            System.out.println("The file does not exist");
        }
        FileUtils.deleteQuietly(myFile);
        if (myFile.exists()) {
            System.out.println("The file exists");
        } else {
            System.out.println("The file does not exist");
        }
    }
}
```
