---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；Power BI；パワーバイ；クエリサービスに接続；
solution: Experience Platform
title: Power BI をクエリサービスに接続します。
topic-legacy: connect
description: このドキュメントでは、Power BIとAdobe Experience Platformクエリサービスを接続する手順について説明します。
exl-id: 8fcd3056-aac7-4226-a354-ed7fb8fe9ad7
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 6%

---

# [!DNL Power BI]をクエリサービス(PC)に接続

このドキュメントでは、Power BIをAdobe Experience Platformクエリサービスに接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL Power BI]へのアクセス権が既にあり、そのインターフェイスの操作方法に精通していることを前提としています。 [!DNL Power BI]に関する詳細は、[正式な [!DNL Power BI] ドキュメント](https://docs.looker.com/)を参照してください。
>
> また、Power BIは&#x200B;****&#x200B;のみで、Windowsデバイスで使用できます。

Power BIをインストールした後は、PostgreSQL用の.NETドライバパッケージ`Npgsql`をインストールする必要があります。 Npgsqlに関する詳細は、[Npgsqlのドキュメント](https://www.npgsql.org/doc/index.html)を参照してください。

>[!IMPORTANT]
>
>v4.0.10以降はダウンロードする必要があります。新しいバージョンではエラーが発生します。

カスタムセットアップ画面の「[!DNL Npgsql GAC Installation]」で、「**[!DNL Will be installed on local hard drive]**」を選択します。

npgsqlが正しくインストールされていることを確認するには、次の手順に進む前にコンピュータを再起動してください。

## [!DNL Power BI]を[!DNL Query Service]に接続

[!DNL Power BI]を[!DNL Query Service]に接続するには、[!DNL Power BI]を開き、上部のメニューリボンで&#x200B;**[!DNL Get Data]**&#x200B;を選択します。

![](../images/clients/power-bi/open-power-bi.png)

**[!DNL PostgreSQL database]**&#x200B;を選択し、次に&#x200B;**[!DNL Connect]**&#x200B;を選択します。

![](../images/clients/power-bi/get-data.png)

これで、サーバーとデータベースの値を入力できます。 データベース名、ホスト、ポート、ログイン資格情報の検索について詳しくは、[Platform の資格情報ページ](https://platform.adobe.com/query/configuration)を参照してください。資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;を選択してから、**[!UICONTROL 資格情報]**&#x200B;を選択します。

**[!DNL Server]** は、接続の詳細の下に見つかったホストです。実稼働環境では、ホスト文字列の末尾に`:80`ポートを追加します。 **[!DNL Database]** には、「all」またはデータセットテーブル名を指定できます。

また、**[!DNL Data Connectivity mode]**&#x200B;を選択することもできます。 **[!DNL Import]**&#x200B;を選択して使用可能なすべてのテーブルのリストを表示するか、**[!DNL DirectQuery]**&#x200B;を選択して直接クエリを作成します。

**[!DNL Import]**&#x200B;モードの詳細については、[プレビューと読み込みの](#preview)の節をお読みください。 **[!DNL DirectQuery]**&#x200B;モードの詳細については、[SQL文の作成](#create)の節をお読みください。 データベースの詳細を確認した後、**[!DNL OK]**&#x200B;を選択します。

![](../images/clients/power-bi/connectivity-mode.png)

ユーザー名、パスワード、アプリケーション設定を求めるプロンプトが表示されます。 次の手順に進むには、これらの詳細を入力し、**[!DNL Connect]**&#x200B;を選択します。

![](../images/clients/power-bi/import-mode.png)

## テーブル{#preview}のプレビューとインポート

**[!DNL Import]**&#x200B;モードを選択した場合は、使用可能なすべてのテーブルのリストを示すダイアログが表示されます。 プレビューするテーブルを選択し、続けて&#x200B;**[!DNL Load]**&#x200B;を選択してデータセットを[!DNL Power BI]に取り込みます。

![](../images/clients/power-bi/preview-table.png)

これで、テーブルがPower BIに読み込まれます。

![](../images/clients/power-bi/import-table.png)

## SQL文の作成{#create}

**[!DNL DirectQuery]**&#x200B;モードを選択した場合は、作成するSQLクエリを[アドバンスオプション]セクションに入力する必要があります。

**[!DNL SQL statement]**&#x200B;の下に、作成するSQLクエリを挿入します。 **[!DNL Include relationship columns]**&#x200B;というラベルの付いたチェックボックスが選択されていることを確認します。 クエリを書き終えたら、**[!DNL OK]**&#x200B;を選択して続行します。

![](../images/clients/power-bi/direct-query-mode.png)

クエリのプレビューが表示されます。 **[!DNL Load]**&#x200B;を選択して、クエリの結果を表示します。

![](../images/clients/power-bi/preview-direct-query.png)

## 次の手順

[!DNL Query Service]に接続したので、[!DNL Power BI]を使ってクエリを書くことができます。 クエリの書き込みと実行の方法の詳細については、[実行中のクエリ](../best-practices/writing-queries.md)のガイドを参照してください。
