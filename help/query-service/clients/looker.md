---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Lookerに接続
topic: connect
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 0%

---


# Lookerに接続

Adobe Experience Platform上でLookerをAdobeクエリサービスに接続するには、次の手順に従ってください。

Lookerにログインした後、「 **Admin**」をクリックし、「 **Connections**」をクリックします。

![](../images/clients/looker/click-admin-connections.png)

このページで、「 **新規接続**」をクリックします。

![](../images/clients/looker/click-new-connection.png)

ここから、「接続設定」の詳細を入力できます。

![](../images/clients/looker/new-connection.png)

- **名前：** 接続の名前。
- **方言：** SQLデータベースに使用されるダイアレクト。 クエリサービスは **PostgreSQLを使用し**&#x200B;ます。
- **ホストとポート：** クエリサービス用のホストエンドポイントとそのポートです。
- **データベース：** 使用するデータベースです。
- **ユーザー名とパスワード：** 使用するログイン資格情報。 ユーザー名は、の形式になり `ORG_ID@AdobeOrg`ます。

>[!NOTE]
>
>ホストとポート、データベース名、ログイン資格情報の検索について詳しくは、Platformの [資格情報ページを参照してください](https://platform.adobe.com/query/configuration)。 資格情報を探すには、Platformにログインし、「 **クエリ**」をクリックし、「 **資格情報**」をクリックします。

接続の詳細を入力したら、「 **Test These Settings** 」をクリックして、秘密鍵証明書が正しく動作することを確認します。 接続した場合は、次に接続可能であることを示すメッセージが表示されます。 接続が成功した場合は、[ **追加接続** ]をクリックして接続を作成します。

![](../images/clients/looker/click-test-connection.png)

## 次の手順

クエリサービスに接続したので、Lookerを使用してクエリを作成できます。 クエリの書き込みおよび実行方法の詳細については、『 [実行中のクエリ』ガイドを参照してください](../creating-queries/creating-queries.md)。