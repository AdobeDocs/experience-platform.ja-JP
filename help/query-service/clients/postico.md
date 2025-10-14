---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；query service;postico;Postico;query service への接続；
solution: Experience Platform
title: クエリサービスへの Postico の接続
description: このドキュメントには、Adobe Experience Platform クエリサービスのバックアップクライアント Postico をインストールするためのリンクが含まれています。
exl-id: a19abfc8-b431-4e57-b44d-c6130041af4a
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 4%

---

# クエリサービス（Mac）への [!DNL Postico] の接続

このドキュメントでは、[!DNL Postico] とAdobe Experience Platform [!DNL Query Service] を接続する手順について説明します。

>[!NOTE]
>
> このガイドは、[!DNL Postico] へのアクセス権を既に持ち、そのインターフェイスの操作方法に精通していることを前提としています。 [!DNL Postico] について詳しくは、[official [!DNL Postico] documentation](https://eggerapps.at/postico/docs) を参照してください。
> 
> さらに、[!DNL Postico] はmacOS デバイスで **のみ** 使用できます。

[!DNL Postico] をクエリサービスに接続するには、[!DNL Postico] を開き、「**[!DNL New Favorite]**」を選択します。 接続設定のダイアログが表示されます。 ここから、Adobe Experience Platformに接続するパラメーター値を入力できます。 以下の接続設定を入力します。 [Postico を使用して PostgreSQL サーバに接続する &#x200B;](https://eggerapps.at/postico/docs/v1.5.21/favorite-window.html) 方法は、公式の Postico Web サイトからも参照できます。

| 接続パラメーター | 説明 |
|---|---|
| **[!DNL Host]:** | PostgreSQL サーバーのホスト名。 |
| **[!DNL Port]:** | [!DNL Query Service] のポート。 [!DNL Query Service] に接続するには、ポート **80** または **5432** を使用する必要があります。 |
| **[!DNL User]** | 特定の接続の名前を作成します。 Macのログイン名を使用する場合は、フィールドを空白のままにします。 |
| **[!DNL Password]** | この英数字の文字列は、Experience Platform **[!UICONTROL パスワード]** 資格情報です。 有効期限のない認証情報を使用する場合、この値は `technicalAccountID` からの連結引数と、設定 JSON ファイルでダウンロードされた `credential` です。 パスワードの値は {technicalAccountId}:{credential} 形式で指定します。 有効期限のない資格情報用の設定 JSON ファイルは、Adobeがコピーを保持しない、初期化中の 1 回限りのダウンロードです。 |
| **[!DNL Database]** | Experience Platform **[!UICONTROL Database]** 資格情報の値 `prod:all` を使用します。 |

データベース名、ホスト、ポートおよびログイン資格情報の検索について詳しくは、[&#x200B; 資格情報ガイド &#x200B;](../ui/credentials.md) を参照してください。 資格情報を見つけるには、[!DNL Experience Platform] にログインし、「**[!UICONTROL クエリ]**」を選択してから、「**[!UICONTROL 資格情報]** を選択します。

資格情報を挿入したら、「**[!DNL Connect]**」を選択してクエリサービスに接続します。

Experience Platformに接続すると、クエリサービスで以前に作成したすべてのリレーションのリストが表示されます。

## SQL 文の作成

新しい SQL クエリを作成するには、サイドバーから「**[!DNL SQL Query]**」を選択します。 または、キーボードショートカット（⇧⌘T）を使用してクエリビューに移動し、実行するクエリを入力します。 終了したら、「**[!DNL Execute Statement]**」を選択してクエリを実行します。 完了したクエリ実行の結果を示すテーブルが表示されます。

[&#x200B; クエリビューの使用 &#x200B;](https://eggerapps.at/postico/docs/v1.3.1/sql-query-view.html) について詳しくは、公式の Postico ドキュメントを参照してください。

## 次の手順

[!DNL Query Service] に接続したので、[!DNL Postico] を使用してクエリを記述できます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
