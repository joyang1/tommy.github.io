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


在近期的项目中，遇到了一个问题 ，就是关于通过SharePoint Client API 去获取office35里面的用户信息的问题。
 
项目的前期都是使用同一个方法，也没有遇到任何问题，但是使用一下方法的前提是需要在SharePoint App的AppManifest.xml文件中配置如下权限：

```sh
<AppPermissionRequest Scope="http://sharepoint/search" Right="QueryAsUserIgnoreAppPrincipal" />
```

然后使用如下方法

## 方法一 SearchAccountNamesByKeyWord

```sh
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

```

项目中的时候一直使用的是刚方法，也没有任何问题。但是在App开发完成之后，准备像微软应用商店提交的时候，微软对App的权限是有限制，所以要求我们降低权限，上面的那个是需要Tenant的权限。

于是我们开始在MSDN上找其它的API来实现这个问题，但是要降低权限。于是我找到Utility这个类里面的方法可以search到user,但是缺点是这个类里面的SearchPrincipals方法只能按字母顺序查
询，意思就是只能按字母的顺序去匹对。但是可以完成上面的功能，search 到office 365里面的user。

## 方法二 GetPrincipalInfos

```sh

public IList<PrincipalInfo> GetPrincipalInfos(String key)
{
    return this.Process(() =>
    {
        using (var clientContext = this.CreateClientContext())
        {
            var userInfoList = Utility.SearchPrincipals(clientContext, clientContext.Web, key,
                PrincipalType.User,
                PrincipalSource.UserInfoList, null, Int32.MaxValue);
            var principalInfos = Utility.SearchPrincipals(clientContext, clientContext.Web, key,
                PrincipalType.User,
                PrincipalSource.All, null, Int32.MaxValue);
            clientContext.ExecuteQuery();
            userInfoList.ForEach(item => principalInfos.Add(item));
            return principalInfos;
        }
    });
}

```

由于以上的只能按字母的顺序去匹对查询，感觉不太好，于是找到微软自己在sharepoint的里面的官方people picker，问题才得以最终解决，以后也希望大家都使用该方法去实现people picker。
github上的官方代码demo:  [Core.PeoplePicker](https://github.com/OfficeDev/PnP/tree/master/Components/Core.PeoplePicker) 

## 方法三 SearchUsers

```sh

public String SearchUsers(String key)
{
    return this.Process(() =>
    {
        using (var clientContext = this.CreateClientContext())
        {
            var param = new ClientPeoplePickerQueryParameters
            {
                QueryString = key,
                AllowEmailAddresses = true,
                AllowMultipleEntities = false,
                PrincipalSource = PrincipalSource.All,
                PrincipalType = PrincipalType.User,
                MaximumEntitySuggestions = rowLimit,
                Required = true
            };
            var result = ClientPeoplePickerWebServiceInterface.ClientPeoplePickerSearchUser(clientContext,
                param);
            clientContext.ExecuteQuery();
            return result.Value;
        }
    });
}


```


下面介绍的这种方法只能search到内部user,不能Search到external user。 所以最终我们也抛弃了这种方法

## 方法四 GetUsers

```sh
public ListItemCollection GetUsers(String key)
{
    return this.Process(() =>
    {
        using (var clientContext = this.CreateClientContext())
        {
            var query = new CamlQuery
            {
                ViewXml = @"<View Scope='RecursiveAll'>
                                <Query>
                                    <Where>
                                            <Contains>
                                                <FieldRef Name='Name'/>
                                                <Value Type='Text'>{0}</Value>
                                            </Contains>
                                    </Where>
                                </Query>
                                <RowLimit>{1}</RowLimit>
                            </View>".FormatWith(key, rowLimit)
            };
            var siteUsers = clientContext.Web.SiteUserInfoList.GetItems(query);
            clientContext.Load(siteUsers);
            clientContext.ExecuteQuery();
            return siteUsers;
        }
    });
}

```

最后，希望这边博客可以帮助大家对SharePoint 里面的people picker 操作起到帮助!!!

有问题随时联系，大家一起学习。