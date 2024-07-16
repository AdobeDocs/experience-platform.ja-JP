---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Looker;looker;query service への接続；
solution: Experience Platform
title: Looker をクエリサービスに接続
description: このドキュメントでは、Looker とAdobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 806e9077-533a-4546-b5ca-8124751957f5
source-git-commit: b059a0191fbf2c3e5d2dfceb9802e2aaa429f7ed
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 7%

---

# クエリサービスへの [!DNL Looker] の接続

このドキュメントでは、[!DNL Looker] とAdobe Experience Platform [!DNL Query Service] を接続する手順について説明します。

>[!NOTE]
>
> このガイドは、[!DNL Looker] へのアクセス権を既に持ち、そのインターフェイスの操作方法に精通していることを前提としています。 [!DNL Looker] について詳しくは、[official [!DNL Looker] documentation](https://docs.looker.com/) を参照してください。

## 新しいデータベース接続の作成 {#create-connection}

[!DNL Looker] にログインしたら、「**[!DNL Admin]**」を選択し、続いて「**[!DNL Connections]**」を選択します。 [!DNL Connections] ページが開きます。 [!DNL Connections] ページで「**[!DNL Add Connection]**」を選択します。

ここから、以下に示す接続設定の詳細を入力します。 [ 新しいデータベース接続を作成する手順と使用可能なプロパティの説明 ](https://cloud.google.com/looker/docs/connecting-to-your-db#creating_a_new_database_connection) については、公式の Looker ドキュメントを参照してください。

- **[!DNL Name]:** 接続名。
- **[!DNL Dialect]:** SQL データベースに使用するダイアレクト。 [!DNL Query Service] は **[!DNL PostgreSQL]** を使用します。
- **[!DNL Host and Port]:** ホストのエンドポイントと [!DNL Query Service] のポート。
- **[!DNL Database]:** 使用するデータベース。
- **[!DNL Username and Password]:** 使用するログイン資格情報。 ユーザー名は、`ORG_ID@AdobeOrg` の形式で入力します。
- **SSL**: SSL を有効にして、ネットワーク全体で安全な接続を確保します。

Looker をクエリサービスに接続するために必要な資格情報を見つけるには、Platform UI にログインし、左側のナビゲーションから **[!UICONTROL クエリ]** を選択し、続いて **[!UICONTROL 資格情報]** を選択します。 **host**、**port**、**database**、**username** および **password** 資格情報の検索について詳しくは、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。

![ 資格情報と期限切れになる資格情報がハイライト表示されているExperience Platformクエリワークスペースの「資格情報」ページ ](../images/clients/looker/query-service-credentials-page.png)

>[!IMPORTANT]
>
>[!DNL Query Service] では、有効期限のない資格情報も提供され、サードパーティのクライアントとの 1 回限りのセットアップが可能になります。 詳しくは、ドキュメント [ 有効期限のない資格情報の生成と使用の方法に関する完全な手順 ](../ui/credentials.md#non-expiring-credentials) を参照してください。 Looker を 1 回限りの設定として接続する場合は、このプロセスを完了する必要があります。 取得した `credential` 値と `technicalAccountId` 値は、Looker `password` パラメーターの値で構成されます。

Adobe Experience Platformでのサードパーティ接続用 SSL サポートについては、[[!DNL Query Service] SSL ドキュメント ](./ssl-modes.md) を参照してください。 このドキュメントでは、SSL モードを使用して接続する方法 `verify-full` 説明します。

接続の詳細を入力したら、「**[!DNL Test These Settings]**」を選択して、資格情報が正しく機能することを確認します。 [ 接続設定のテスト ](https://cloud.google.com/looker/docs/connecting-to-your-db#testing_your_connection_settings) について詳しくは、公式の Looker ドキュメントを参照してください。 接続に成功すると、接続できることを示すメッセージが画面に表示されます。 接続に成功したら、「**[!DNL Add Connection]**」を選択して接続を作成します。

## 次の手順

[!DNL Query Service] に接続したので、[!DNL Looker] を使用してクエリを記述できます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
