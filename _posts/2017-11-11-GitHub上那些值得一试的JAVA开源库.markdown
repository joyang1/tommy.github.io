---
layout:     post
title:      "GitHub上那些值得一试的JAVA开源库"
subtitle:   " \"JAVA开源库\""
date:       2017-11-11 20:43:13
author:     "Tommy"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Java
    - JAVA开源库
    - 开源库
---


作为一名程序员，你几乎每天都会使用到GitHub上的那些著名Java第三方库，比如Apache Commons,Spring，Hibernate等等。除了这些，你可能还会fork或Star一些其他的开源库，但GitHub上的库实在太多了，以至于对于个人来说，你很难有时间去发现并了解那些不断加入的新库，而它们却往往能在一些新兴领域中给你提供帮助。

我一直使用JAVA来写后端应用，平时也会关注一些国外技术大牛的博客（来自Tapki、DZone、Google Developer等技术博客），从而注意到了一些新的而且很有意思Java开源库，它们有些能给你的项目带来帮助，有些是以游戏的形式帮你提高Java的编程水平，而另一些则能够帮助你识别JAVA程序中的常见问题 。在这多达330,000个JAVA开源库中，我收集了下面这些或许也值得你一试的Java开源库。

## Strman-java - 字符串处理
[链接地址](https://github.com/shekhargulati/strman-java)

Strmen-java是一个字符串处理工具，你可以通过maven将它引入到项目中。除了Java本身的字符串处理方式外，我们还可以使用Apache Common Langs里的StringUtils来简化String的操作。但以上两种方式对于我们日常编程中最容易碰到的字符串处理来说，仍然显得有些不足。Strmen-java为我们提供了一个非常完整且强大的解决方案，使用它可以解决几乎所有字符串处理场景。

下面便是Strman-java的几个常见使用示例：

拼接字符串

```
import static strman.Strman.append
append("f", "o", "o", "b", "a", "r")
// result => "foobar"
```

获取某一个位置的字符

```
import static strman.Strman.at
at("foobar", 0)
// result => Optional("f")
```

取出某两个字符包含的内容

```
import static strman.Strman.between
between("[abc][def]", "[", "]")
```

Base64 编码

```
import static strman.Strman.base64Encode
base64Encode("strman")
// result => "c3RybWFu"
```

## Tablesaw - “大数据”
[链接地址](https://github.com/jtablesaw/tablesaw)

谈到大数据，我们想到的总是Hodoop加上集群部署，但有没有一种更小巧的方式，能让我们在单机上方便地实现大数据的那些功能呢？Tablesaw给我们提供了一种基于内存的高性能大数据解决方案。你可以使用它的API方便地从RDBMS或是CSV中导入数据，然后利用Tablesaw提供的接口对数据进行排序、筛选、分组、map/reduce等操作。

根据文档给出的说明，你将可以在22秒内将500,000,000行（每行4个字段）的数据文件加载到10G的内存中。而查询速度更是达到仅需1-2ms。

## Dex - 数据可视化
[链接地址](https://github.com/PatMartin/Dex)

Dex是一个数据可视化解决方案，它支持超过50种不同的视图类型，包括世界地图，timeline，3D图形等等。Dex是使用Java/JavaFX编写的，你将可以很方便地将它与你的其他程序整合（比如用R语言写的大数据分析程序）创造出美观的图表来。
<img src = "/img/kaiyuankuangjia/kaiyuan5.png">


## Bootique - 微服务框架
[链接地址](https://github.com/bootique/bootique)

以前开发Web应用程序时，我们总需要先构建一个应用，然后将它打包（war），再部署到如Tomcat这样的Web容器中。但随着微服务架构的流行，我们需要更轻量化，非容器的开发框架。SpringBoot是我一直在使用的，而Bootique无疑是另一种优秀的选择。它允许你通过具有不同功能的模块插入，来支持如REST Service，Web app，定时调度，数据迁移等功能。而使用它写的程序都则会被打包为一个Jar文件，你可以通过命令行更灵活地去启动它。

从很多角度看，它都很像SpringBoot，将你从Java应用从它所依赖的Web容器中解放出来，程序员们可以有更强的自主性，去写主程序的main()函数。甚至在你不添加任何额外的模块的情况下，你也能直接使用Bootqiue去实现一个Java应用。

## Gumshoe - Java程序检测
[链接地址](https://github.com/worstcase/gumshoe)

Gumshoe是一个JAVA程序检测工具，它能帮助你跟踪程序的负载和性能。它能通过度量TCP,UDP,CPU使用等信息，帮助你分析出资源的使用情况 ，同时它也提供了Java程序中调用栈的分析功能，比如提供某个方法调用的次数，频度等信息。
<img src = "/img/kaiyuankuangjia/kaiyuan.png">

## LeakCanary - 内存泄漏监控
[链接地址](https://github.com/square/leakcanary)

内存泄漏一直是令Java程序员苦恼的问题，因为在你开发阶段很难察觉内存泄漏问题，而一旦到了生产环境，则可能因为它而造成严重的后果。LeakCanary是一个内存泄漏检查工具，只需要像下面这样简单加入LeakCanary，它便能全程监控你的应用，并在出现内存泄漏时给你发出警告。LeakCanary同时支持Android和Java，下面是在Android应用中使用的例子。
<img src = "/img/kaiyuankuangjia/kaiyuan1.png">

```
public class ExampleApplication extends Application {

  @Override public void onCreate() {
    super.onCreate();
    LeakCanary.install(this);
  }
}
```

## awesome-java - JAVA资源大集合
[链接地址](https://github.com/akullpp/awesome-java)

Awesome-java得到了14608个Star，作者将JAVA中那些最常用的第三方库按照分类整理成了一个列表。包含Ancients(古老，但常用的)，Bean Mapping，Build，Bytecode Manipulation，Code Analysis，Command-line Argument Parsers，Configuration，Continuous Integration，CSV，Database等等，简直是一本jiava第三方库大全，如果你对项目中应该使用哪一个库不确定，或希望选择几个库来做比较，都可以到awesome-java上进行参考。
<img src = "/img/kaiyuankuangjia/kaiyuan3.png">

## 99-Problems - 学习JAVA8 
[链接地址](https://github.com/shekhargulati/99-problems)

99-Problems是一个很有意思的GitHub项目，它对三种不同的语言Java 8,Scala和Haskell分别提出了99个问题，让你通过使用特定语言编程来提供一个最优的解决方案。

这些问题分为不同的难度等级，用*表示，一个星号表示在15分钟内解决，2个星号可能需要30-69分钟，而最难的3个星号，则需要更长时间（90分钟左右），如果你能在限定的时间内使用JAVA8的特性解决所有的问题，那说明你对JAVA8的掌握程度已经非常牢固了。如果你没办法解决所有问题也没关系，你可以查看作者提供的代码示例，这也是你学习JAVA8很好的途径。

## Chronicle Map - 高效键值对存储
[链接地址](https://github.com/OpenHFT/Chronicle-Map)

Chronicle Map是一个基于内存的键值对存储方案。以其低延迟、高并发的特性著称，并在交易及金融系统中得到应用。另外，他还支持持久化到磁盘，以及多键值查询的功能。

下面是官方文档中一段对于从JAVA角度描述Chronicle Map的说明：

>From Java perspective, ChronicleMap is a ConcurrentMap implementation which stores the entries off-heap, serializing/deserializing key and value objects to/from off-heap memory transparently. Chronicle Map supports

> 1.Key and value objects caching/reusing for making zero allocations (garbage) on queries.
> 2.Flyweight values for eliminating serialization/deserialization cost and allowing direct read/write access to off-heap memory.

## ND4J - 科学计算
[链接地址](http://nd4j.org/)

ND4J是一个开源的数值计算扩展 ，它将 Python中著名的numpy库的很多特性带到了Java中。ND4J可以用来存储和处理大型多维矩阵。它的计算和处理速度很快，但占用的内存却很少，程序员们可以很容易地使用它来与其他JAVA或Scala库作接口。
ND4J主要包括了：一个强大的N维数组对象Array,比较成熟的函数库；实用的线性代数、傅里叶变换和随机数生成函数等。它可以与Hadoop或者Spark这样的工具整合使用。
<img src = "/img/kaiyuankuangjia/kaiyuan4.png">


## Automon - Java监控
[链接地址](https://github.com/stevensouza/automon)

Automon是一个非常灵活的JAVA监控工具，它结合了AOP(AspectJ)以及JDK和其他依赖库的功能特性，以声明方式去监控你的Java代码。它可以与JAMon，JavaSimon，Yammer Metrics，StatsD和像 perf4j,log4j,sl4j这样的logging库结合使用。
Automon最常被用于跟踪Java方法的调用时长，异常次数等信息，并在你选择的工具中显示监控结果。它并不自己进行任何监控动作，但却很好地扮演了“我应该监控什么”以及“我如何进行监控”这两者之间中间人的角色。而且它的安装也非常简单，只需要简单进行配置便可使用。
<img src = "/img/kaiyuankuangjia/kaiyuan2.png">

## Swiss Java Knife - JAVA工具集
[链接地址](https://github.com/aragozin/jvm-tools)

SJK（Java瑞士军刀）是一个用于JVM监控、排错以及调优的工具集。它是一个命令行工具，但使用起来非常方便，你可以用它来查询JVM中线程的CPU使用，GC实时信息，以及基本调优选项。也可以结合MBean以JSON格式导出所有你需要的信息。


以上只是GitHub中那些优秀开源库的冰山一角，作为一名现代的Java程序员，你除了需要优秀的编程能力之外，善于发现并使用那些优秀的开源库将使你更上一个台阶。如果你也有好的Java开源库推荐，请在下面留言，我会补充道这份清单中，让更多JAVA程序员能够从中受益。

此篇博客转自简书:http://www.jianshu.com/p/ad40e6dd3789
注：以上开源库来自Tapki的推荐。
