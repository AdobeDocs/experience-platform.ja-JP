---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: RStudioに接続
topic: connect
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 2%

---


# RStudioに接続

このドキュメントでは、R StudioとAdobe Experience Platformクエリサービスを接続する手順について説明します。

RStudioをインストールした後、表示される *コンソール* 画面で、PostgreSQLを使用するためのRスクリプトを準備する必要があります。

```r
install.packages("RPostgreSQL")
install.packages("rstudioapi")
require("RPostgreSQL")
require("rstudioapi")
```

PostgreSQLを使用するためのRスクリプトを準備したら、PostgreSQLドライバを読み込むことで、RStudioをクエリサービスに接続できるようになりました。

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
| `{HOST_NUMBER` および `{PORT_NUMBER}` | クエリサービス用のホストエンドポイントとそのポートです。 |
| `{USERNAME}` および `{PASSWORD}` | 使用するログイン資格情報。 ユーザー名は、の形式をとり `ORG_ID@AdobeOrg`ます。 |

>[!NOTE]
>
>データベース名、ホスト、ポート、ログインの資格情報の検索について詳しくは、Platformの [資格情報ページを参照してください](https://platform.adobe.com/query/configuration)。 資格情報を探すには、Platformにログインし、「 **クエリ**」をクリックし、「 **資格情報**」をクリックします。

## 次の手順

クエリサービスに接続したら、SQL文を実行および編集するクエリを作成できます。 たとえば、を使用してクエリ `dbGetQuery(con, sql)` を実行できます。ここで、 `sql` は実行するSQLクエリです。

次のクエリでは、ExperienceEventsを含むデータセットを使用し [て](../creating-queries/experience-event-queries.md) 、デバイスの画面の高さが決まると、Webサイトのページ表示のヒストグラムを作成します。

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

成功した応答は、クエリの結果を返します。

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

クエリの書き込みおよび実行方法の詳細については、『 [実行中のクエリ』ガイドを参照してください](../creating-queries/creating-queries.md)。