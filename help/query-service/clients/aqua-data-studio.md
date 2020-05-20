---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Aqua Data Studioに接続
topic: connect
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# Aqua Data Studioに接続

このドキュメントでは、Aqua Data StudioとAdobe Experience Platformクエリサービスを接続する手順について説明します。

Aqua Data Studioのインストール後、まずサーバーを登録する必要があります。 メインメニューで「 **Server**」をクリックし、「 **Register Server**」をクリックします。

![](../images/clients/aqua-data-studio/register-server.png)

「 *Register Server* 」ダイアログが表示されます。 「 *一般* 」タブで、左側のリストから **PostgreSQL** を選択します。 表示されるダイアログで、サーバー設定の次の詳細を指定します。

- **名前**: 接続の名前。
- **Login Name and Password**: 使用するログイン資格情報。 ユーザー名は、の形式をとり `ORG_ID@AdobeOrg`ます。
- **ホストとポート**: クエリサービス用のホストエンドポイントとそのポートです。
- **データベース：** 使用するデータベースです。

>[!NOTE] ログイン資格情報、ホスト、ポートおよびデータベース名の検索について詳しくは、Platformの [資格情報ページを参照してください](https://platform.adobe.com/query/configuration)。 資格情報を探すには、Platformにログインし、「 **クエリ**」をクリックし、「 **資格情報**」をクリックします。

![](../images/clients/aqua-data-studio/register-server-general-tab.png)

Select the **Driver** tab. [ *パラメータ*]で、値を `?sslmode=require`

![](../images/clients/aqua-data-studio/register-server-driver-tab.png)

接続の詳細を入力したら、「接続を **テスト** 」をクリックして、秘密鍵証明書が正しく動作することを確認します。 接続に成功した場合は、「 **保存** 」をクリックしてサーバーを登録します。 登録が成功すると *ダッシュボードに接続が表示され* 、サーバーに接続でき、スキーマオブジェクトの表示が完了したことを確認します。

## 次の手順

クエリサービスに接続したら、Aqua Data Studio内の *クエリアナライザ* （Aca Data Studio内）を使用してSQL文を実行および編集できます。 クエリの書き込みおよび実行方法の詳細については、『 [実行中のクエリ』ガイドを参照してください](../creating-queries/creating-queries.md)。