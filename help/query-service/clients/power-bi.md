---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；クエリサービス；Power BI;power bi；クエリサービスへの接続；
solution: Experience Platform
title: Power BI をクエリサービスに接続します。
topic-legacy: connect
description: このドキュメントでは、Adobe Experience PlatformクエリサービスとPower BIを接続する手順について説明します。
exl-id: 8fcd3056-aac7-4226-a354-ed7fb8fe9ad7
source-git-commit: 910a38ccb556ec427584d9b522e29f6877d1c987
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 2%

---

# [!DNL Power BI] をクエリサービス (PC) に接続

このドキュメントでは、Adobe Experience PlatformクエリサービスとPower BIを接続する手順を説明します。

>[!NOTE]
>
> このガイドは、既に [!DNL Power BI] にアクセスでき、インターフェイスの操作方法に精通していることを前提としています。 [!DNL Power BI] の詳細については、[ 公式の  [!DNL Power BI]  ドキュメント ](https://docs.microsoft.com/ja-JP/power-bi/) を参照してください。
>
> また、Power BIは **Windows デバイスでのみ** 使用可能です。

Power BIをインストールした後、PostgreSQL 用の.NET ドライバパッケージ `Npgsql` をインストールする必要があります。 Npgsql に関する詳細は、[Npgsql のドキュメント ](https://www.npgsql.org/doc/index.html) を参照してください。

>[!IMPORTANT]
>
>新しいバージョンではエラーが発生するので、v4.0.10 以前をダウンロードする必要があります。

カスタムセットアップ画面の「[!DNL Npgsql GAC Installation]」で、「**[!DNL Will be installed on local hard drive]**」を選択します。

npgsql が正しくインストールされていることを確認するには、次の手順に進む前にコンピューターを再起動してください。

## [!DNL Power BI] を [!DNL Query Service] に接続

[!DNL Power BI] を [!DNL Query Service] に接続するには、[!DNL Power BI] を開き、上部のメニューリボンで **[!DNL Get Data]** を選択します。

![](../images/clients/power-bi/open-power-bi.png)

**[!DNL PostgreSQL database]** を選択し、その後に **[!DNL Connect]** を選択します。

![](../images/clients/power-bi/get-data.png)

これで、サーバーとデータベースの値を入力できます。 データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。 資格情報を探すには、[!DNL Platform] にログインし、**[!UICONTROL クエリ]** を選択してから、**[!UICONTROL 資格情報]** を選択します。

**[!DNL Server]** は、接続の詳細で見つかったホストです。実稼動環境の場合は、ホスト文字列の末尾にポート `:80` を追加します。 **[!DNL Database]** は、「すべて」またはデータセットテーブル名です。

さらに、**[!DNL Data Connectivity mode]** を選択することもできます。 **[!DNL Import]** を選択して使用可能なすべてのテーブルのリストを表示するか、**[!DNL DirectQuery]** を選択してクエリを直接作成します。

**[!DNL Import]** モードの詳細については、[ テーブルのプレビューと読み込み ](#preview) の節を参照してください。 **[!DNL DirectQuery]** モードの詳細については、[SQL 文の作成 ](#create) の節を参照してください。 データベースの詳細を確認した後、**[!DNL OK]** を選択します。

![](../images/clients/power-bi/connectivity-mode.png)

ユーザー名、パスワード、アプリケーションの設定を求めるプロンプトが表示されます。 次の手順に進むには、この詳細を入力し、**[!DNL Connect]** を選択します。

![](../images/clients/power-bi/import-mode.png)

## テーブルのプレビューとインポート {#preview}

**[!DNL Import]** モードを選択すると、ダイアログが表示され、使用可能なすべてのテーブルのリストが表示されます。 プレビューするテーブルを選択し、**[!DNL Load]** を選択してデータセットを [!DNL Power BI] に取り込みます。

![](../images/clients/power-bi/preview-table.png)

これで、テーブルがPower BIにインポートされます。

![](../images/clients/power-bi/import-table.png)

## SQL 文の作成 {#create}

**[!DNL DirectQuery]** モードを選択した場合は、作成する SQL クエリを「詳細オプション」セクションに入力する必要があります。

**[!DNL SQL statement]** の下に、作成する SQL クエリを挿入します。 **[!DNL Include relationship columns]** というラベルの付いたチェックボックスが選択されていることを確認します。 クエリを作成したら、**[!DNL OK]** を選択して続行します。

![](../images/clients/power-bi/direct-query-mode.png)

クエリのプレビューが表示されます。 **[!DNL Load]** を選択して、クエリの結果を表示します。

![](../images/clients/power-bi/preview-direct-query.png)

## 次の手順

[!DNL Query Service] に接続したら、[!DNL Power BI] を使用してクエリを記述できます。 クエリの書き込みと実行の詳細については、[ クエリの実行 ](../best-practices/writing-queries.md) に関するガイドを参照してください。
