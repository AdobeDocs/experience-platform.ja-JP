---
keywords: Experience Platform;home;popular topics;Query service;query service;RStudio;rstudio;connect to query service;
solution: Experience Platform
title: RStudio との接続
topic: connect
translation-type: tm+mt
source-git-commit: c5d3be4706ca6d6a30e203067db6ddc894b9bfb4
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 61%

---


# Connect with [!DNL RStudio]

This document walks through the steps for connecting R Studio with Adobe Experience Platform [!DNL Query Service].

After installing [!DNL RStudio], on the *Console* screen that appears, you will first need to prepare your R script to use [!DNL PostgreSQL].

```r
install.packages("RPostgreSQL")
install.packages("rstudioapi")
require("RPostgreSQL")
require("rstudioapi")
```

Once you have prepared your R script to use [!DNL PostgreSQL], you can now connect [!DNL RStudio] to [!DNL Query Service] by loading the [!DNL PostgreSQL] driver.

```r
drv <- dbDriver("PostgreSQL")
con <- dbConnect(drv, 
 dbname = "{DATABASE_NAME}",
 host="{HOST_NUMBER}",
 port={PORT_NUMBER},
 user="{USERNAME}",
 password="{PASSWORD}")
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{DATABASE_NAME}` | 使用するデータベースの名前。 |
| `{HOST_NUMBER` および `{PORT_NUMBER}` | ホストエンドポイントと、そのクエリサービス用のポート。 |
| `{USERNAME}` および `{PASSWORD}` | 使用されるログイン資格情報。ユーザー名の形式は `ORG_ID@AdobeOrg` となります。 |

>[!NOTE]
>
>データベース名、ホスト、ポート、ログイン資格情報の検索について詳しくは、[Platform の資格情報ページ](https://platform.adobe.com/query/configuration)を参照してください。To find your credentials, log in to [!DNL Platform], click **[!UICONTROL Queries]**, then click **[!UICONTROL Credentials]**.

## 次の手順

Now that you have connected to [!DNL Query Service], you can write queries to execute and edit SQL statements. たとえば、`dbGetQuery(con, sql)` を使用してクエリを実行できます。ここで、`sql` は実行する SQL クエリです。

次のクエリでは、[ExperienceEvents](../creating-queries/experience-event-queries.md) を含むデータセットを使用し、デバイスの画面の高さを指定して、Web サイトのページビューのヒストグラムを作成します。

```sql
df_pageviews <- dbGetQuery(con,
"SELECT t.range AS buckets, 
 Count(*) AS pageviews 
FROM (SELECT CASE 
 WHEN device.screenheight BETWEEN 0 AND 99 THEN '0 - 99' 
 WHEN device.screenheight BETWEEN 100 AND 199 THEN '100-199' 
 WHEN device.screenheight BETWEEN 200 AND 299 THEN '200-299' 
 WHEN device.screenheight BETWEEN 300 AND 399 THEN '300-399' 
 WHEN device.screenheight BETWEEN 400 AND 499 THEN '400-499' 
 WHEN device.screenheight BETWEEN 500 AND 599 THEN '500-599' 
 ELSE '600-699' 
 end AS range 
 FROM aa_post_vals_3) t 
GROUP BY t.range 
ORDER BY buckets 
LIMIT 1000000")
```

成功応答は、クエリの結果を返します。

```r
df_pageviews
 buckets pageviews
1 0 - 99 198985
2 500-599 67138
3 300-399 2147
4 200-299 354
5 400-499 6947
6 100-199 4415
7 600-699 3097040
```

クエリの書き込みおよび実行方法の詳細については、[クエリの実行ガイド](../creating-queries/creating-queries.md)を参照してください。