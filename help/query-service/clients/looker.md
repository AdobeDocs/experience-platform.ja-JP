---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Lookerに接続
topic: connect
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# 接続先 [!DNL Looker]

Adobe Experience Platform [!DNL Looker] に接続するに [!DNL Query Service] は、次の手順に従ってください。

ログイン後、「 [!DNL Looker]Admin **[!UICONTROL 」をクリックし、「]** Connections ****」をクリックします。

![](../images/clients/looker/click-admin-connections.png)

このページで、「 **新規接続**」をクリックします。

![](../images/clients/looker/click-new-connection.png)

ここから、「接続設定」の詳細を入力できます。

![](../images/clients/looker/new-connection.png)

- **名前：** 接続の名前。
- **方言：** SQLデータベースに使用されるダイアレクト。 [!DNL Query Service] の使用 **[!DNL PostgreSQL]**&#x200B;例
- **ホストとポート：** のホストエンドポイントとそのポート [!DNL Query Service]。
- **データベース：** 使用するデータベースです。
- **ユーザー名とパスワード：** 使用するログイン資格情報。 ユーザー名は、の形式になり `ORG_ID@AdobeOrg`ます。

>[!NOTE]
>
>ホストとポート、データベース名、ログイン資格情報の検索について詳しくは、Platformの [資格情報ページを参照してください](https://platform.adobe.com/query/configuration)。 資格情報を探すには、にログインし、「 [!DNL Platform]クエリ **[!UICONTROL 」をクリックし、「]**&#x200B;資格情報 ****」をクリックします。

接続の詳細を入力したら、「 **[!UICONTROL Test These Settings]** 」をクリックして、秘密鍵証明書が正しく動作することを確認します。 接続した場合は、次に接続可能であることを示すメッセージが表示されます。 接続が成功した場合は、[ **[!UICONTROL 追加接続]** ]をクリックして接続を作成します。

![](../images/clients/looker/click-test-connection.png)

## 次の手順

に接続したら、を使用してクエリを書くこ [!DNL Query Service]と [!DNL Looker] ができます。 クエリの書き込みおよび実行方法の詳細については、『 [実行中のクエリ』ガイドを参照してください](../creating-queries/creating-queries.md)。