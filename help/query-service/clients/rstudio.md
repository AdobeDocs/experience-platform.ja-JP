---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；RStudio;rstudio；クエリサービスへの接続；
solution: Experience Platform
title: RStudio をクエリサービスに接続
topic-legacy: connect
description: このドキュメントでは、R Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 8dd82bad-6ffb-4536-9c27-223f471a49c6
source-git-commit: 9ab3d69553dee9fdb97472edfa3f812133ee1bb1
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 11%

---

# 接続 [!DNL RStudio] クエリサービスへ

このドキュメントでは、 [!DNL RStudio] Adobe Experience Platform [!DNL Query Service].

>[!NOTE]
>
> このガイドは、ユーザーが既に [!DNL RStudio] そしてその使い方に精通している 詳細情報： [!DNL RStudio] は [公式 [!DNL RStudio] ドキュメント](https://rstudio.com/products/rstudio/).
> 
> また、クエリサービスで RStudio を使用するには、PostgreSQL JDBC 4.2 ドライバーをインストールする必要があります。 JDBC ドライバーは、 [PostgreSQL 公式サイト](https://jdbc.postgresql.org/download/).

## の作成 [!DNL Query Service] 接続 [!DNL RStudio] インターフェイス

インストール後 [!DNL RStudio]RJDBC パッケージをインストールする必要があります。 次に移動： **[!DNL Packages]** ウィンドウ枠で、 **[!DNL Install]**.

![](../images/clients/rstudio/install-package.png)

ポップアップが表示され、 **[!DNL Install Packages]** 画面 以下を確認します。 **[!DNL Repository (CRAN)]** が **[!DNL Install from]** 」セクションに入力します。 の値 **[!DNL Packages]** は、 `RJDBC`. 確認 **[!DNL Install dependencies]** が選択されている。 すべての値が正しいことを確認したら、「 」を選択します。 **[!DNL Install]** をクリックして、パッケージをインストールします。

![](../images/clients/rstudio/install-jrdbc.png)

RJDBC パッケージがインストールされたので、RStudio を再起動して、インストールプロセスを完了します。

RStudio が再起動した後、クエリサービスに接続できるようになりました。 を選択します。 **[!DNL RJDBC]** パッケージを **[!DNL Packages]** ウィンドウに移動し、コンソールに次のコマンドを入力します。

```console
pgsql <- JDBC("org.postgresql.Driver", "{PATH TO THE POSTGRESQL JDBC JAR}", "`")
```

ここで、{PATH TO THE POSTGRESQL JDBC JAR} は、お使いのコンピューターにインストールされた PostgreSQL JDBC JAR へのパスを表します。

これで、コンソールで次のコマンドを入力して、クエリサービスへの接続を作成できます。

```console
qsconnection <- dbConnect(pgsql, "jdbc:postgresql://{HOSTNAME}:{PORT}/{DATABASE_NAME}?user={USERNAME}&password={PASSWORD}&sslmode=require")
```

>[!IMPORTANT]
>
>詳しくは、 [[!DNL Query Service] SSL ドキュメント](./ssl-modes.md) を参照して、Adobe Experience Platform Query Service へのサードパーティ接続の SSL サポートと、 `verify-full` SSL モード。

データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、 [資格情報ガイド](../ui/credentials.md). 資格情報を検索するには、にログインします。 [!DNL Platform]を選択し、「 **[!UICONTROL クエリ]**&#x200B;に続いて **[!UICONTROL 資格情報]**.

![](../images/clients/rstudio/connection-rjdbc.png)

## クエリの記述

これで、 [!DNL Query Service]を使用すると、SQL 文を実行および編集するクエリを記述できます。 たとえば、`dbGetQuery(con, sql)` を使用してクエリを実行できます。ここで、`sql` は実行する SQL クエリです。

次のクエリでは、 [エクスペリエンスイベント](../sample-queries/experience-event.md) およびは、デバイスの画面の高さを指定して、Web サイトのページビューのヒストグラムを作成します。

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

クエリの書き込みと実行の方法について詳しくは、 [クエリの実行](../best-practices/writing-queries.md).
