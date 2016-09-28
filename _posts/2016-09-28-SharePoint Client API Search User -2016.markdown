---
layout:     post
title:      "SharePoint Client API Search user  四种方法"
subtitle:   " \"SharePoint Client API Search user\""
date:       2016-09-26 12:41:00
author:     "Tommy"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - SharePoint
    - SharePoint Client API
    - Search user
---

> “Let's Start. ”

# SharePoint Client API Search user  四种方法

在近期的项目中，遇到了一个问题 ，就是关于通过SharePoint Client API 去获取office35里面的用户信息的问题。
 
项目的前期都是使用同一个方法，也没有遇到任何问题，但是使用一下方法的前提是需要在SharePoint App的AppManifest.xml文件中配置如下权限：
<AppPermissionRequest Scope="http://sharepoint/search" Right="QueryAsUserIgnoreAppPrincipal" />

然后使用如下方法：
## 方法一： SearchAccountNamesByKeyWord
public List<String> SearchAccountNamesByKeyWord(String key)
{
    return this.Process(() =>
    {
        using (var clientContext = this.CreateClientContext())
        {
            var keywordQuery = new KeywordQuery(clientContext);
            keywordQuery.QueryText = String.Format("{0}*", key);
            keywordQuery.SourceId = new Guid("B09A7990-05EA-4AF9-81EF-EDFAB16C4E31");
            keywordQuery.RowLimit = rowLimit;
            keywordQuery.TrimDuplicates = false;

            var searchExecutor = new SearchExecutor(clientContext);
            var results = searchExecutor.ExecuteQuery(keywordQuery);
            clientContext.ExecuteQuery();
            var accounts = new List<String>();
            foreach (var resultTable in results.Value)
            {
                foreach (var row in resultTable.ResultRows)
                {
                    accounts.Add(row[accoutNameKeyName] as string);
                }
            }
            return accounts;
        }
    });
}

项目中的时候一直使用的是刚方法，也没有任何问题。但是在App开发完成之后，准备像微软应用商店提交的时候，微软对App的权限是有限制，所以要求我们降低权限，上面的那个是需要Tenant的权限。

于是我们开始在MSDN上找其它的API来实现这个问题，但是要降低权限
