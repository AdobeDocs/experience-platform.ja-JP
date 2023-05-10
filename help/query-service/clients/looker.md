---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；Looker;looker；クエリサービスへの接続；
solution: Experience Platform
title: Looker をクエリサービスに接続
description: このドキュメントでは、Looker とAdobe Experience Platform Query Service を接続する手順について説明します。
exl-id: 806e9077-533a-4546-b5ca-8124751957f5
source-git-commit: b059a0191fbf2c3e5d2dfceb9802e2aaa429f7ed
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 8%

---

# クエリサービスへの [!DNL Looker] の接続

このドキュメントでは、 [!DNL Looker] Adobe Experience Platform [!DNL Query Service].

>[!NOTE]
>
> このガイドは、ユーザーが既に [!DNL Looker] インターフェイスの操作方法を熟知している。 詳細情報： [!DNL Looker] は [公式 [!DNL Looker] ドキュメント](https://docs.looker.com/).

## 新しいデータベース接続を作成 {#create-connection}

へのログイン後 [!DNL Looker]を選択します。 **[!DNL Admin]**&#x200B;に続いて **[!DNL Connections]**. この [!DNL Connections] ページが開きます。 の [!DNL Connections] ページ、選択 **[!DNL Add Connection]**.

ここから、以下に示す接続設定の詳細を入力します。 詳しくは、 Looker の公式ドキュメントを参照してください。 [新しいデータベース接続を作成する手順と使用可能なプロパティの説明](https://cloud.google.com/looker/docs/connecting-to-your-db#creating_a_new_database_connection).

- **[!DNL Name]:** 接続の名前。
- **[!DNL Dialect]:** SQL データベースで使用される言語。 [!DNL Query Service] 使用する **[!DNL PostgreSQL]**.
- **[!DNL Host and Port]:** ホストエンドポイントと、そののポート [!DNL Query Service].
- **[!DNL Database]**：使用するデータベース。
- **[!DNL Username and Password]:** 使用されるログイン資格情報。 ユーザー名は、`ORG_ID@AdobeOrg` の形式で入力します。
- **SSL**:SSL を有効にして、ネットワークを介した安全な接続を確保します。

Looker とクエリサービスの接続に必要な資格情報を見つけるには、Platform UI にログインし、「 」を選択します。 **[!UICONTROL クエリ]** 左のナビゲーションから、の後に **[!UICONTROL 資格情報]**. の検索方法の詳細 **ホスト**, **ポート**, **データベース**, **ユーザー名**、および **パスワード** 認証情報、お読みください [資格情報ガイド](../ui/credentials.md).

![「認証情報クエリ」ワークスペースの「Experience Platform」ページ（「認証情報」と「有効期限」がハイライト表示されています）。](../images/clients/looker/query-service-credentials-page.png)

>[!IMPORTANT]
>
>[!DNL Query Service] また、は、期限切れでない資格情報を提供して、サードパーティクライアントとの 1 回限りのセットアップを可能にします。 詳しくは、 [有効期限のない資格情報の生成および使用方法に関する完全な手順](../ui/credentials.md#non-expiring-credentials). Looker を 1 回限りのセットアップとして接続する場合は、このプロセスを完了する必要があります。 この `credential` および `technicalAccountId` 取得された値が Looker の値を構成します `password` パラメーター。

Adobe Experience Platformでのサードパーティ接続の SSL サポートについて詳しくは、 [[!DNL Query Service] SSL ドキュメント](./ssl-modes.md). このドキュメントでは、 `verify-full` SSL モード。

接続の詳細を入力したら、「 」を選択します。 **[!DNL Test These Settings]** 認証情報が正しく機能するようにします。 詳細情報： [接続設定のテスト](https://cloud.google.com/looker/docs/connecting-to-your-db#testing_your_connection_settings) は、Looker の公式ドキュメントに記載されています。 接続が成功すると、接続できることを示すメッセージが画面に表示されます。 接続に成功したら、「 」を選択します。 **[!DNL Add Connection]** 接続を作成します。

## 次の手順

これで、 [!DNL Query Service]を使用する場合、 [!DNL Looker] クエリを書き込みます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
