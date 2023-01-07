---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；Looker;looker；クエリサービスへの接続；
solution: Experience Platform
title: Looker をクエリサービスに接続
description: このドキュメントでは、Looker とAdobe Experience Platform Query Service を接続する手順について説明します。
exl-id: 806e9077-533a-4546-b5ca-8124751957f5
source-git-commit: 58eadaaf461ecd9598f3f508fab0c192cf058916
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 10%

---

# 接続 [!DNL Looker] クエリサービスへ

このドキュメントでは、 [!DNL Looker] Adobe Experience Platform [!DNL Query Service].

>[!NOTE]
>
> このガイドは、ユーザーが既に [!DNL Looker] インターフェイスの操作方法を熟知している。 詳細情報： [!DNL Looker] は [公式 [!DNL Looker] ドキュメント](https://docs.looker.com/).

へのログイン後 [!DNL Looker]を選択します。 **[!DNL Admin]**&#x200B;に続いて **[!DNL Connections]**.

![この [!DNL Looker] 管理者ドロップダウンメニューで強調表示された接続を含むダッシュボード。](../images/clients/looker/click-admin-connections.png)

このページで、 **[!DNL New Connection]**.

![[ 新しい接続 ] がハイライト表示された [ 接続 ] ワークスペース。](../images/clients/looker/click-new-connection.png)

ここから、接続設定の詳細を入力できます。

![新しい接続の接続設定ページ。](../images/clients/looker/new-connection.png)

- **[!DNL Name]:** 接続の名前。
- **[!DNL Dialect]:** SQL データベースで使用される言語。 [!DNL Query Service] 使用する **[!DNL PostgreSQL]**.
- **[!DNL Host and Port]:** ホストエンドポイントと、そののポート [!DNL Query Service].
- **[!DNL Database]**：使用するデータベース。
- **[!DNL Username and Password]:** 使用されるログイン資格情報。 ユーザー名は、`ORG_ID@AdobeOrg` の形式で入力します。
- **SSL**:SSL を有効にして、ネットワークを介した安全な接続を確保します。

>[!IMPORTANT]
>
>詳しくは、 [[!DNL Query Service] SSL ドキュメント](./ssl-modes.md) を参照して、Adobe Experience Platform Query Service へのサードパーティ接続の SSL サポートと、 `verify-full` SSL モード。

ホストとポート、データベース名、ログイン資格情報の検索の詳細については、 [資格情報ガイド](../ui/credentials.md). 資格情報を検索するには、にログインします。 [!DNL Platform]を選択し、「 **[!UICONTROL クエリ]**&#x200B;に続いて **[!UICONTROL 資格情報]**.

接続の詳細を入力したら、「 」を選択します。 **[!DNL Test These Settings]** 認証情報が正しく機能するようにします。 接続できる場合は、接続できることを示すメッセージが下に表示されます。 接続に成功した場合は、 **[!DNL Add Connection]** 接続を作成します。

![[ 新しい接続 ] の [ 接続の設定 ] ページで、[ テストする ] 設定がハイライト表示されています。](../images/clients/looker/click-test-connection.png)

## 次の手順

これで、 [!DNL Query Service]を使用する場合、 [!DNL Looker] クエリを書き込みます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
