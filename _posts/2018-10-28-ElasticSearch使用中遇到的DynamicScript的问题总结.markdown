---
layout:     post
title:      "ElasticSearch-DynamicScript使用总结"
subtitle:   " \"DynamicScript使用总结\""
date:       2018-10-28 12:05:13
author:     "Tommy"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Java
    - ElasticSearch
---

## 问题描述

最近工程中会使用到ElasticSearch(以下统称ES),就是将一些统计结果(点击量:click_count,曝光量:impr_count,点击曝光比:ctr=click_count/impr_count)写入到ES,会用到ES的dynamic script去实时修改ctr。然后就遇到了too many dynamic script rejected的问题。

## 问题解决过程


