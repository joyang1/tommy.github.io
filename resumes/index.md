---
layout: resumes
title: 个人简历
pdf: https://blog.tommyyang.cn/resume/2019-resume.pdf
nav: false
---
<br><br><br>
# 联系方式

- QQ：1173870805
- Email：tingzai.yang@gmail.com

---

# 个人信息

 - 杨挺/男/1994
 - 本科/大连工业大学
 - 工作年限：3年
 - 技术博客：[https://blog.tommyyang.cn](https://blog.tommyyang.cn)
 - Github：[tommyyang](https://github.com/joyang1)
 - 期望职位：大数据工程师/高级Java工程师/推荐服务工程师
 - 期望薪资：**税后**月薪26k左右

---

# 技能水平
- 熟练使用 Java 进行服务端、微服务开发。
- 熟悉 JVM 调优。
- 熟悉 ElasticSearch，可以基于 ElasticSearch 进行线上服务开发。
- 熟悉 Kibana 使用，可以基于 Kibana 进行数据分析。
- 熟悉大数据开发，可以使用 Hadoop，Hive 进行离线任务的计算与优化。
- 熟悉 HBase，Kafka，RabbitMQ 等，可以利用其进行实时任务、事件的开发。
- 熟练使用 Apache Flink，可以基于 Flink 连接 Hive、HBase 进行离线数据的统计分析。
- 可以基于 Flink 连接 Kafka、RabbitMQ 等进行实时系统的计算与开发。
- 熟练使用 SpringMVC、Spring、Mybatis 进行 Web 开发。
- 熟悉 Quartz 等定时任务框架，可以基于 Quartz 进行定时任务开发。
- 熟悉 C 语言特性与开发。
- 熟悉 Docker 流程，可以将项目进行 Docker 化。
- 熟悉 Python Scrapy 爬虫，可以基于 Python 构建爬虫系统。

# 开源项目
- 基于 Java 的 Redis BloomFilter 的客户端工具：[JRebloom](https://github.com/RedisLabs/JReBloom)
- 基于 Java 的 Json Logger 第三方库：[JsonLogger4Java](https://github.com/joyang1/slf4j4json)
- Java 知识总结：[JavaInterview](https://github.com/joyang1/JavaInterview)
- 基于 Golang 的 GraphQL API 服务：[Email-Dashboard](https://github.com/Email-Dashboard/Email-Dashboard)

---

# 工作经历  

## 上海佰集信息科技有限公司[简书](https://www.jianshu.com) （2018.6-至今）
### 职位：服务端工程师

>工作描述

负责推荐部门相关工作，包括推荐接口优化、文章页相关文章推荐、推送离线计 算工具、用户画像实时计算系统的项目构建、项目优化与完善;   
通过推荐系统接口优化和实时画像系统上线，将智能推荐文章的 CTR 由原来的 6% 提升到了 14%。

相关文章通过智能推荐优化将 CTR 从 0.9%提高到 3%;上升的原因主要就是策略 的改变，由之前的keyword关键字搜索改成了智能化推荐，用到的策略是doc2vec 及 user2vec，最后对召回的文章进行排序，借鉴的是知乎推荐的方式，由刚开始 的多策略召回，然后过滤掉给用户推荐过、用户看过的文章(使用的业界用的比较 多的过滤方式，Redis BloomFilter，从海量数据里面找出需要的元素)，最后按用 户行为、文章特征进行排序，筛选出最优的相关推荐文章。

熟悉推荐系统中 A/B Test 的整个流程，可以基于 A/B Test 提高推荐系统的评判参数（CTR，文章满意阅读数等）。

推送离线计算工具解决推荐服务效率慢的问题(由于推荐和推送使用的是同一个 线上排序服务，后来我将推送这块业务重构成了一个离线的 Job)

推荐系统使用 golang 重构。通过推荐系统的开发，熟悉了使用 golang 开发微服务项目的整个流程；可以基于 golang 实现微服务项目的构建；熟练使用远程服务调用框架 grpc。

用户画像实时计算系统使用 Apache Flink 构建。通过该项目的构建，可以熟练地使用 Apache Flink 对实时流数据的计算和统计、以及实时计算系统的构建和部署。

涉及到的技术包括:JVM，Java，RxJava，Hadoop，Hive，HBase，Spark，Kafka，Flink，golang，ElasticSearch，Redis，grpc 等。

推荐算法相关技术: XGBoost，doc2vec，user2vec，followship，LDA，长短期画像等。


## 上海聚力传媒科技有限公司[PPTV](http://www.pptv.com)  （2017.8-2018.6）
###  职位:大数据 Java 开发工程师

> 工作描述   

负责PP视频移动客户端自动化推送功能开发。
推送集成的第三方推送平台，开始使用的个推，后面使用自己公司的云信平台(接入的是友盟)。
推送的对象就是PP视频移动端的所有用户。
涉及到的技术包括:SpringMVC，Scala，Hadoop，Hive，HBase，Spark，Kafka，RabbitMQ，Quartz等。
自动化推送，包括很多场景，通过后台对各场景推送报文进行配置，各场景是否启用推送，推送对象，推送时间等进行配置，然后实现自动化推送。   

## 上海径点软件科技有限公司  （2015.10-2017.8）
###  职位: 软件研发工程师

> 工作描述   

上海径点软件科技有限公司，负责维护和更新公司 Azure 云平台、Office 365 产品 后台的开发以及产品客户问题的处理。Azure 云平台的维护以及更新，平台上暂 时有 9 个产品，最好的两个产品的盈利大概在 100 万美元。
负责维护的 Azure 云平台。vCloud 是依托于 Microsoft ® Azure®云计算平台的软件 即服务(SAAS)平台。 该平台上有自己的安全子系统，管理子系统，跟踪子系统。我工作的主要内容就 是维护该平台以及相关的子系统，负责开发的子系统有安全子系统和管理子系 统。
除了维护以及开发更新 vCloud 平台，也负责开发 Office 365 App，其中主要是 SharePoint App 的开发。

---

# 获奖经历
- 2015.05 获得辽宁省计算机设计竞赛三等奖
- 2015.04 获得第六届“蓝桥杯”全国软件大赛-软件 Java 组省级三等奖
- 2014.11 获得大连工业大学科技创新奖
- 2014.11 获得大连工业大学“科技创新”奖学金
- 2014.08 获得第一届辽宁省移动应用大赛三等奖
- 2014.06 获得第五届“蓝桥杯”全国软件大赛-软件 C/C++组全国优秀奖
- **2014.04 获得第五届“蓝桥杯”全国软件大赛-软件 C/C++组省级一等奖**
- **2014.05 获得辽宁省计算机设计竞赛一等奖**
- 2013.10 获得 2013 年全国大学生数学建模竞赛辽宁赛区二等奖
- **2013.06 获得大连市第二十二界数学竞赛一等奖**

---

# 致谢
感谢您百忙之中抽出时间来阅读我的简历，期待能有机会与您共事，一起探讨技术。
