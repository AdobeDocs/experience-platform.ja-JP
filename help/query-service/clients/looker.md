---
keywords: Experience Platform;home;popular topics;Query service;query service;Looker;looker;connect to query service;
solution: Experience Platform
title: Looker との接続
topic: connect
description: このドキュメントでは、LookerとAdobe Experience Platformクエリサービスを接続する手順について説明します。
translation-type: tm+mt
source-git-commit: 4b2df39b84b2874cbfda9ef2d68c4b50d00596ac
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 66%

---


# Connect with [!DNL Looker]

To connect [!DNL Looker] with [!DNL Query Service] on Adobe Experience Platform, please follow the steps below:

After logging into [!DNL Looker], click on **[!UICONTROL Admin]**, followed by **[!UICONTROL Connections]**.

![](../images/clients/looker/click-admin-connections.png)

このページで、「**新しい接続**」をクリックします。

![](../images/clients/looker/click-new-connection.png)

ここから、接続設定の詳細を入力できます。

![](../images/clients/looker/new-connection.png)

- **名前：**&#x200B;接続の名前。
- **言語：** SQL データベースで使用される言語。[!DNL Query Service] の使用 **[!DNL PostgreSQL]**&#x200B;例
- **ホストとポート：** のホストエンドポイントとそのポート [!DNL Query Service]。
- **データベース：**&#x200B;使用するデータベース。
- **ユーザー名とパスワード：**&#x200B;使用するログイン資格情報。ユーザー名は、`ORG_ID@AdobeOrg` の形式で入力します。

>[!NOTE]
>
> ホストとポート、データベース名、ログイン資格情報の検索について詳しくは、[Platform の資格情報](https://platform.adobe.com/query/configuration)ページを参照してください。To find your credentials, log in to [!DNL Platform], click **[!UICONTROL Queries]**, then click **[!UICONTROL Credentials]**.

接続の詳細を入力したら、「**[!UICONTROL Test These Settings]**」をクリックし、資格情報が正しく機能することを確認します。正しく機能している場合は、接続が成功したこと示すメッセージが下に表示されます。接続が成功した場合は、「**[!UICONTROL 接続を追加]**」をクリックして接続を作成します。

![](../images/clients/looker/click-test-connection.png)

## 次の手順

Now that you&#39;ve connected with [!DNL Query Service], you can use [!DNL Looker] to write queries. クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../creating-queries/creating-queries.md)』を参照してください。