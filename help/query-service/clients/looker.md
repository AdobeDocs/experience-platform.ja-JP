---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；ルッカー；ルッカー；クエリサービスに接続；
solution: Experience Platform
title: Lookerをクエリサービスに接続
topic-legacy: connect
description: このドキュメントでは、LookerとAdobe Experience Platformクエリサービスを接続する手順について説明します。
exl-id: 806e9077-533a-4546-b5ca-8124751957f5
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 22%

---

# [!DNL Looker]をクエリサービスに接続

このドキュメントでは、[!DNL Looker]をAdobe Experience Platform[!DNL Query Service]に接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL Looker]へのアクセス権が既にあり、そのインターフェイスの操作方法に精通していることを前提としています。 [!DNL Looker]に関する詳細は、[正式な [!DNL Looker] ドキュメント](https://docs.looker.com/)を参照してください。

[!DNL Looker]にログインした後、**[!DNL Admin]**&#x200B;を選択し、次に&#x200B;**[!DNL Connections]**&#x200B;を選択します。

![](../images/clients/looker/click-admin-connections.png)

このページで、**[!DNL New Connection]**&#x200B;を選択します。

![](../images/clients/looker/click-new-connection.png)

ここから、接続設定の詳細を入力できます。

![](../images/clients/looker/new-connection.png)

- **[!DNL Name]:** 接続名。
- **[!DNL Dialect]:** SQLデータベースで使用される方言。[!DNL Query Service] の使用 **[!DNL PostgreSQL]**&#x200B;例
- **[!DNL Host and Port]:** のホストエンドポイントとそのポート [!DNL Query Service]。
- **[!DNL Database]**：使用するデータベース。
- **[!DNL Username and Password]:** 使用するログイン資格情報。ユーザー名は、`ORG_ID@AdobeOrg` の形式で入力します。

>[!NOTE]
>
> ホストとポート、データベース名、ログイン資格情報の検索について詳しくは、[Platform の資格情報](https://platform.adobe.com/query/configuration)ページを参照してください。資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;を選択してから、**[!UICONTROL 資格情報]**&#x200B;を選択します。

接続の詳細を入力したら、**[!DNL Test These Settings]**&#x200B;を選択して、秘密鍵証明書が正しく機能することを確認します。 接続できる場合は、接続できることを示すメッセージが下に表示されます。 接続が成功した場合は、**[!DNL Add Connection]**&#x200B;を選択して接続を作成します。

![](../images/clients/looker/click-test-connection.png)

## 次の手順

[!DNL Query Service]に接続したので、[!DNL Looker]を使ってクエリを書くことができます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
