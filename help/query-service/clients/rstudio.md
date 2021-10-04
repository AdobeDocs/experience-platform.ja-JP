---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；RStudio;rstudio；クエリサービスへの接続；
solution: Experience Platform
title: RStudio をクエリサービスに接続
topic-legacy: connect
description: このドキュメントでは、R Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 8dd82bad-6ffb-4536-9c27-223f471a49c6
source-git-commit: 910a38ccb556ec427584d9b522e29f6877d1c987
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 12%

---

# [!DNL RStudio] をクエリサービスに接続

このドキュメントでは、[!DNL RStudio] とAdobe Experience Platform [!DNL Query Service] を接続する手順について説明します。

>[!NOTE]
>
> このガイドは、既に [!DNL RStudio] にアクセスでき、使い方に精通していることを前提としています。 [!DNL RStudio] の詳細については、[ 公式の  [!DNL RStudio]  ドキュメント ](https://rstudio.com/products/rstudio/) を参照してください。
> 
> また、クエリサービスで RStudio を使用するには、PostgreSQL JDBC 4.2 ドライバーをインストールする必要があります。 [PostgreSQL 公式サイト ](https://jdbc.postgresql.org/download.html) から JDBC ドライバをダウンロードできます。

## [!DNL RStudio] インターフェイスで [!DNL Query Service] 接続を作成する

[!DNL RStudio] をインストールした後、RJDBC パッケージをインストールする必要があります。 **[!DNL Packages]** ウィンドウに移動し、**[!DNL Install]** を選択します。

![](../images/clients/rstudio/install-package.png)

ポップアップが表示され、**[!DNL Install Packages]** 画面が表示されます。 **[!DNL Install from]** セクションで **[!DNL Repository (CRAN)]** が選択されていることを確認します。 **[!DNL Packages]** の値は `RJDBC` にする必要があります。 **[!DNL Install dependencies]** が選択されていることを確認します。 すべての値が正しいことを確認したら、**[!DNL Install]** を選択してパッケージをインストールします。

![](../images/clients/rstudio/install-jrdbc.png)

RJDBC パッケージがインストールされたら、RStudio を再起動してインストールプロセスを完了します。

RStudio が再起動した後、クエリサービスに接続できるようになりました。 **[!DNL Packages]** ウィンドウで **[!DNL RJDBC]** パッケージを選択し、コンソールで次のコマンドを入力します。

```console
pgsql <- JDBC("org.postgresql.Driver", "{PATH TO THE POSTGRESQL JDBC JAR}", "`")
```

ここで、{PATH TO THE POSTGRESQL JDBC JAR} は、お使いのコンピューターにインストールされた PostgreSQL JDBC JAR へのパスを表します。

これで、コンソールで次のコマンドを入力して、クエリサービスへの接続を作成できます。

```console
qsconnection <- dbConnect(pgsql, "jdbc:postgresql://{HOSTNAME}:{PORT}/{DATABASE_NAME}?user={USERNAME}&password={PASSWORD}&sslmode=require")
```

>[!NOTE]
>
>データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。 資格情報を探すには、[!DNL Platform] にログインし、**[!UICONTROL クエリ]** を選択してから、**[!UICONTROL 資格情報]** を選択します。

![](../images/clients/rstudio/connection-rjdbc.png)

## クエリの記述

[!DNL Query Service] に接続したら、SQL 文を実行および編集するクエリを書き込むことができます。 たとえば、`dbGetQuery(con, sql)` を使用してクエリを実行できます。ここで、`sql` は実行する SQL クエリです。

次のクエリでは、[ エクスペリエンスイベント ](../best-practices/experience-event-queries.md) を含むデータセットを使用し、デバイスの画面の高さを指定して、Web サイトのページビューのヒストグラムを作成します。

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

クエリの書き込みと実行の詳細については、[ クエリの実行 ](../best-practices/writing-queries.md) に関するガイドを参照してください。
