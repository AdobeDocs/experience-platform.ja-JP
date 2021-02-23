---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；RStudio;rstudio;クエリサービスに接続；
solution: Experience Platform
title: RStudioをクエリサービスに接続
topic: connect
description: このドキュメントでは、R Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
translation-type: tm+mt
source-git-commit: f1b2fd7efd43f317a85c831cd64c09be29688f7a
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 18%

---


# [!DNL RStudio]をクエリサービスに接続

このドキュメントでは、[!DNL RStudio]とAdobe Experience Platform[!DNL Query Service]を結ぶ手順を順を追って説明します。

>[!NOTE]
>
> このガイドは、[!DNL RStudio]へのアクセス権が既にあり、その使い方に精通していることを前提としています。 [!DNL RStudio]に関する詳細は、[正式な [!DNL RStudio] ドキュメント](https://rstudio.com/products/rstudio/)を参照してください。
> 
> さらに、クエリサービスでRStudioを使用するには、PostgreSQL JDBC 4.2ドライバーをインストールする必要があります。 JDBCドライバは[PostgreSQL公式サイト](https://jdbc.postgresql.org/download.html)からダウンロードできます。

## [!DNL RStudio]インターフェイスに[!DNL Query Service]接続を作成する

[!DNL RStudio]をインストールした後、RJDBCパッケージをインストールする必要があります。 **[!DNL Packages]**&#x200B;ウィンドウに移動し、**[!DNL Install]**&#x200B;を選択します。

![](../images/clients/rstudio/install-package.png)

**[!DNL Install Packages]**&#x200B;画面が表示され、ポップアップが表示されます。 **[!DNL Install from]**&#x200B;セクションで&#x200B;**[!DNL Repository (CRAN)]**&#x200B;が選択されていることを確認します。 **[!DNL Packages]**&#x200B;の値は`RJDBC`にする必要があります。 **[!DNL Install dependencies]**&#x200B;が選択されていることを確認します。 すべての値が正しいことを確認したら、**[!DNL Install]**&#x200B;を選択してパッケージをインストールします。

![](../images/clients/rstudio/install-jrdbc.png)

RJDBCパッケージがインストールされたら、RStudioを再起動してインストールプロセスを完了します。

RStudioを再起動した後、クエリサービスに接続できるようになりました。 **[!DNL Packages]**&#x200B;ウィンドウで&#x200B;**[!DNL RJDBC]**&#x200B;パッケージを選択し、コンソールで次のコマンドを入力します。

```console
pgsql <- JDBC("org.postgresql.Driver", "{PATH TO THE POSTGRESQL JDBC JAR}", "`")
```

{PATH TO THE POSTGRESQL JDBC JAR}は、コンピューターにインストールされたPostgreSQL JDBC JARへのパスを表します。

コンソールで次のコマンドを入力して、クエリサービスへの接続を作成できます。

```console
qsconnection <- dbConnect(pgsql, "jdbc:postgresql://{HOSTNAME}:{PORT}/{DATABASE_NAME}?user={USERNAME}&password={PASSWORD}&sslmode=require")
```

>[!NOTE]
>
>データベース名、ホスト、ポート、ログイン資格情報の検索について詳しくは、[Platform の資格情報ページ](https://platform.adobe.com/query/configuration)を参照してください。資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;を選択してから、**[!UICONTROL 資格情報]**&#x200B;を選択します。

![](../images/clients/rstudio/connection-rjdbc.png)

## クエリの記述

[!DNL Query Service]に接続したので、SQL文を実行および編集するクエリを書き込むことができます。 たとえば、`dbGetQuery(con, sql)` を使用してクエリを実行できます。ここで、`sql` は実行する SQL クエリです。

次のクエリでは、[エクスペリエンスイベント](../best-practices/experience-event-queries.md)を含むデータセットを使用し、デバイスの画面の高さが決まると、Webサイトのページ表示のヒストグラムを作成します。

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

## 次の手順

クエリの書き込みと実行の方法の詳細については、[実行中のクエリ](../best-practices/writing-queries.md)のガイドを参照してください。