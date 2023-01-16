---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；RStudio;rstudio；クエリサービスへの接続；
solution: Experience Platform
title: RStudio をクエリサービスに接続
description: このドキュメントでは、R Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 8dd82bad-6ffb-4536-9c27-223f471a49c6
source-git-commit: d40aa52240ab8f15feea62ec5fb8de073dd6a053
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 10%

---

# 接続 [!DNL RStudio] クエリサービスへ

このドキュメントでは、 [!DNL RStudio] Adobe Experience Platform [!DNL Query Service].

>[!NOTE]
>
> [!DNL RStudio] は現在、 [!DNL Posit]. [!DNL RStudio] 製品名は「 [!DNL Posit Connect], [!DNL Posit Workbench], [!DNL Posit Package] マネージャ [!DNL Posit Cloud]、および [!DNL Posit Academy].
>
> このガイドは、ユーザーが既に [!DNL RStudio] そしてその使い方に精通している 詳細情報： [!DNL RStudio] は [公式 [!DNL RStudio] ドキュメント](https://rstudio.com/products/rstudio/).
> 
> また、 [!DNL RStudio] クエリサービスを使用する場合は、 [!DNL PostgreSQL] JDBC 4.2 ドライバ。 JDBC ドライバーは、 [[!DNL PostgreSQL] 公式サイト](https://jdbc.postgresql.org/download/).

## の作成 [!DNL Query Service] 接続 [!DNL RStudio] インターフェイス

インストール後 [!DNL RStudio]RJDBC パッケージをインストールする必要があります。 方法に関する説明 [コマンドラインを使用してデータベースを接続する](https://solutions.posit.co/connections/db/best-practices/drivers/#connecting-to-a-database-in-r) は公式の Posit ドキュメントに記載されています。

Mac OS を使用している場合は、 **[!UICONTROL ツール]** メニューバーから、 **[!UICONTROL パッケージのインストール]** をドロップダウンメニューから選択します。 または、 **[!DNL Packages]** RStudio UI の「 」タブで、「 」を選択します。 **[!DNL Install]**.

ポップアップが表示され、 **[!DNL Install Packages]** 画面 以下を確認します。 **[!DNL Repository (CRAN)]** が **[!DNL Install from]** 」セクションに入力します。 の値 **[!DNL Packages]** は、 `RJDBC`. 確認 **[!DNL Install dependencies]** が選択されている。 すべての値が正しいことを確認したら、「 」を選択します。 **[!DNL Install]** をクリックして、パッケージをインストールします。 RJDBC パッケージがインストールされたので、を再起動します。 [!DNL RStudio] をクリックして、インストールプロセスを完了します。

後 [!DNL RStudio] が再起動され、クエリサービスに接続できるようになりました。 を選択します。 **[!DNL RJDBC]** パッケージを **[!DNL Packages]** ウィンドウに移動し、コンソールに次のコマンドを入力します。

```console
pgsql <- JDBC("org.postgresql.Driver", "{PATH TO THE POSTGRESQL JDBC JAR}", "`")
```

ここで、 `{PATH TO THE POSTGRESQL JDBC JAR}` は、 [!DNL PostgreSQL] お使いのコンピューターにインストールされた JDBC JAR。

これで、クエリサービスへの接続を作成できます。 コンソールで次のコマンドを入力します。

```console
qsconnection <- dbConnect(pgsql, "jdbc:postgresql://{HOSTNAME}:{PORT}/{DATABASE_NAME}?user={USERNAME}&password={PASSWORD}&sslmode=require")
```

>[!IMPORTANT]
>
>詳しくは、 [[!DNL Query Service] SSL ドキュメント](./ssl-modes.md) を参照して、Adobe Experience Platform Query Service へのサードパーティ接続の SSL サポートと、 `verify-full` SSL モード。

データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、 [資格情報ガイド](../ui/credentials.md). 資格情報を検索するには、にログインします。 [!DNL Platform]を選択し、「 **[!UICONTROL クエリ]**&#x200B;に続いて **[!UICONTROL 資格情報]**.

コンソール出力のメッセージで、クエリサービスへの接続を確認します。

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
