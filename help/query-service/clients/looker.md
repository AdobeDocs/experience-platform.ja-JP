---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Lookerとの接続
topic: connect
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# Lookerとの接続

LookerをAdobe Experience Platform上のAdobeクエリサービスと接続するには、次の手順に従ってください。

Lookerにログインした後、「 **Admin**」をクリックし、「 **Connections**」をクリックします。

![](../images/clients/looker/click-admin-connections.png)

このページで、「新規接続」をク **リックします**。

![](../images/clients/looker/click-new-connection.png)

ここから、接続設定の詳細を入力できます。

![](../images/clients/looker/new-connection.png)

- **名前：** 接続の名前。
- **方言：** SQLデータベースで使用されるダイアレクト。 クエリサービスは **PostgreSQLを使用します**。
- **ホストとポート：** ホストエンドポイントと、そのクエリサービス用のポート。
- **データベース：** 使用するデータベースです。
- **ユーザー名とパスワード：** 使用されるログイン資格情報。 ユーザー名は、の形式で入力します `ORG_ID@AdobeOrg`。

>[!NOTE] ホストとポート、データベース名、ログイン資格情報の検索について詳しくは、プラットフォームの資格情報 [ページを参照してください](https://platform.adobe.com/query/configuration)。 資格情報を検索するには、Platformにログインし、「 **クエリ**」をクリックし、「秘密鍵証明書」をクリ **ックします**。

接続の詳細を入力したら、「次の設定をテスト **」をクリックして** 、秘密鍵証明書が正しく機能することを確認します。 接続した場合は、次のメッセージが表示されます。 接続が成功した場合は、[接続]をクリッ **追加クして** 、接続を作成します。

![](../images/clients/looker/click-test-connection.png)

## 次の手順

これで、クエリサービスに接続したので、Lookerを使用してクエリを作成できます。 クエリの書き込みおよび実行方法の詳細については、『実行中のクエリ』ガイドを [参照してくださ](../creating-queries/creating-queries.md)い。