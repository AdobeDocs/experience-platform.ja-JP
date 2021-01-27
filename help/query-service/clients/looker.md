---
keywords: Experience Platform;home;popular topics;Query service;query service;Looker;looker;connect to query service;
solution: Experience Platform
title: Looker との接続
topic: connect
description: このドキュメントでは、LookerとAdobe Experience Platformクエリサービスを接続する手順について説明します。
translation-type: tm+mt
source-git-commit: 9fbb6b829cd9ddec30f22b0de66874be7710e465
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 66%

---


# [!DNL Looker]に接続

[!DNL Looker]をAdobe Experience Platformの[!DNL Query Service]と接続するには、次の手順に従ってください。

[!DNL Looker]にログインした後、**[!UICONTROL 管理者]**&#x200B;をクリックし、次に&#x200B;**[!UICONTROL 接続]**&#x200B;をクリックします。

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
> ホストとポート、データベース名、ログイン資格情報の検索について詳しくは、[Platform の資格情報](https://platform.adobe.com/query/configuration)ページを参照してください。資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;をクリックしてから、**[!UICONTROL 資格情報]**&#x200B;をクリックします。

接続の詳細を入力したら、「**[!UICONTROL Test These Settings]**」をクリックし、資格情報が正しく機能することを確認します。正しく機能している場合は、接続が成功したこと示すメッセージが下に表示されます。接続が成功した場合は、「**[!UICONTROL 接続を追加]**」をクリックして接続を作成します。

![](../images/clients/looker/click-test-connection.png)

## 次の手順

[!DNL Query Service]に接続したので、[!DNL Looker]を使ってクエリを書くことができます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。