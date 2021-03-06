---
layout:     post
title:      "JsonLogger For Java"
subtitle:   " \"JsonLogger 结合slf4j 实现json格式的log\""
date:       2018-08-29 18:15:13
author:     "Tommy"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Java
    - log4j
    - slf4j 
    - jsonlog
---


# `slf4j4json`

## Description
为slf4j提供自定义json格式的log输出

## maven dependency
#### 添加repository

```
<repositories>
    <repository>
        <id>release</id>
        <url>https://raw.github.com/joyang1/slf4j4json/master/</url>
        <snapshots>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
        </snapshots>
    </repository>
</repositories>
```
#### 添加dependency
``` 
<dependency>
    <groupId>cn.tommyyang</groupId>
    <artifactId>slf4j4json</artifactId>
    <version>1.1</version>
</dependency>
```

### 使用
#### 需要配置log4j.properties
因为该jar包同样是依赖于slf4j,所以按log4j配置log4j.properties即可正常使用
```
log4j.rootLogger=DEBUG,console

#输出日志到控制台
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.Threshold=all
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%-d{yyyy-MM-dd HH\:mm\:ss} [%c\:%L]-[%p] %m%n
```

#### slf4j4json使用
    
- 直接打印

```
//通过LoggerFactory获取一个logger
final static Logger logger = LoggerFactory.getLogger(Test.class);

//打印必须调用log()方法
logger.info().field("key1", "value1").intField("key2", 1).log();
logger.warn().field("key1", "value1").intField("key2", 1).log();
logger.debug().strField("key1", "value1").longField("key2", 1L).log();
logger.trace().strField("key1", "value1").intField("key2", 1).log();
logger.error().strField("key1", "value1").intField("key2", 1).log();
```


- 比如有一些键值对要重复赋值,使用getLogger方法，实例如下

```
JsonLogger jsonLogger = logger.info().strField("appname", "app").getLogger();
jsonLogger.strField("msg","infotest1").intField("port", 2).log();
jsonLogger.field("msg","infotest2").longField("long", 23L).log();
```

## 输出
```
{
    "key1":"value1",
    "key2":1,
    "index":"server-end-log",
    "host":"DESKTOP-2B1VG6J",
    "level":"info",
    "time":1535021174
}
```


## version
- `1.0版本` 2018-08-29
    - 实现JsonLog For Java的基本功能
- `1.1版本` 2018-08-31
    - 修复JsonLogger jsonLogger = logger.info().strField("appname", "app").getLogger(); 出现重复内容的bug

## Github地址
本文内容在[Github](https://github.com/joyang1/slf4j4json){:target="_blank"},欢迎大家fork或star。

