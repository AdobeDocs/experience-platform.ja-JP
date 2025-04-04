---
keywords: Experience Platform；ホーム；人気のトピック；Query Service;Query Service;RStudio;rstudio;Query Service への接続；
solution: Experience Platform
title: クエリサービスへの RStudio の接続
description: このドキュメントでは、R Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 8dd82bad-6ffb-4536-9c27-223f471a49c6
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 11%

---

# クエリサービスへの [!DNL RStudio] の接続

このドキュメントでは、[!DNL RStudio] とAdobe Experience Platform [!DNL Query Service] を接続する手順について説明します。

>[!NOTE]
>
> [!DNL RStudio] は現在、[!DNL Posit] としてリブランドされています。 [!DNL RStudio] 製品の名前は、[!DNL Posit Connect]、[!DNL Posit Workbench]、[!DNL Posit Package] Manager、[!DNL Posit Cloud]、[!DNL Posit Academy] に変更されました。
>
> このガイドは、[!DNL RStudio] へのアクセス権を既に持ち、使用方法に精通していることを前提としています。 [!DNL RStudio] について詳しくは、[official [!DNL RStudio] documentation](https://rstudio.com/products/rstudio/) を参照してください。
> 
> さらに、クエリサービスで [!DNL RStudio] を使用するには、[!DNL PostgreSQL] JDBC 4.2 ドライバーをインストールする必要があります。 JDBC ドライバーは、[[!DNL PostgreSQL]  公式サイト ](https://jdbc.postgresql.org/download/) からダウンロードできます。

## [!DNL RStudio] インターフェイスでの [!DNL Query Service] 接続の作成

[!DNL RStudio] をインストールしたら、RJDBC パッケージをインストールする必要があります。 [ コマンドラインを使用してデータベースを接続する ](https://solutions.posit.co/connections/db/best-practices/drivers/#connecting-to-a-database-in-r) 方法については、公式の Post ドキュメントを参照してください。

Mac OS を使用している場合は、メニューバーから **[!UICONTROL ツール]** を選択し、その後ドロップダウンメニューから **[!UICONTROL パッケージをインストール]** を選択できます。 または、RStudio UI から「**[!DNL Packages]**」タブを選択し、「**[!DNL Install]**」を選択します。

ポップアップが表示され、**[!DNL Install Packages]** 画面が表示されます。 **[!DNL Install from]** セクションに **[!DNL Repository (CRAN)]** が選択されていることを確認します。 **[!DNL Packages]** の値は `RJDBC` にしてください。 「**[!DNL Install dependencies]**」が選択されていることを確認します。 すべての値が正しいことを確認したら、「**[!DNL Install]**」を選択してパッケージをインストールします。 RJDBC パッケージがインストールされたので、[!DNL RStudio] を再起動してインストールプロセスを完了します。

再起動 [!DNL RStudio] たら、クエリサービスに接続できます。 **[!DNL Packages]** ペインで **[!DNL RJDBC]** パッケージを選択し、コンソールに次のコマンドを入力します。

```console
pgsql <- JDBC("org.postgresql.Driver", "{PATH TO THE POSTGRESQL JDBC JAR}", "`")
```

`{PATH TO THE POSTGRESQL JDBC JAR}` は、コンピューターにインストールされた [!DNL PostgreSQL] JDBC JAR へのパスを表します。

これで、クエリサービスへの接続を作成できます。 コンソールに次のコマンドを入力します。

```console
qsconnection <- dbConnect(pgsql, "jdbc:postgresql://{HOSTNAME}:{PORT}/{DATABASE_NAME}?user={USERNAME}&password={PASSWORD}&sslmode=require")
```

>[!IMPORTANT]
>
>Adobe Experience Platform クエリサービスへのサードパーティ接続での SSL サポートと、SSL モードを使用した接続方法については、[[!DNL Query Service] SSL ドキュメント ](./ssl-modes.md) を参照し `verify-full` ください。

データベース名、ホスト、ポートおよびログイン資格情報の検索について詳しくは、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。 資格情報を見つけるには、[!DNL Experience Platform] にログインし、「**[!UICONTROL クエリ]**」を選択してから、「**[!UICONTROL 資格情報]** を選択します。

コンソール出力内のメッセージで、クエリサービスへの接続を確認します。

## クエリの記述

[!DNL Query Service] に接続したので、SQL 文を実行および編集するクエリを記述できます。 たとえば、`dbGetQuery(con, sql)` を使用してクエリを実行できます。ここで、`sql` は実行する SQL クエリです。

次のクエリでは、[ エクスペリエンスイベント ](../../xdm/classes/experienceevent.md) を含むデータセットを使用し、デバイスの画面の高さを考慮して、web サイトのページビューのヒストグラムを作成します。

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

クエリの作成および実行方法について詳しくは、[ クエリの実行 ](../best-practices/writing-queries.md) に関するガイドを参照してください。
