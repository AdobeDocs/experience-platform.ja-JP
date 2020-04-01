---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Aqua Data Studioとの接続
topic: connect
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# Aqua Data Studioとの接続

このドキュメントでは、Aqua Data StudioとAdobe Experience Platform Platform Serviceを接続する手順について説明します。

Aqua Data Studioのインストール後、まずサーバーを登録する必要があります。 メインメニューで「 **Server**」をクリックし、「 **Register Server**」をクリックします。

![](../images/clients/aqua-data-studio/register-server.png)

サーバの *登録ダイアログ* が表示されます。 「一般 *」タブで、左側* のリストから **** 「PostgreSQL」を選択します。 表示されるダイアログで、サーバー設定の次の詳細を指定します。

- **名前**:接続の名前。
- **Login Name and Password**:使用されるログイン資格情報。 ユーザー名は、の形式をとりま `ORG_ID@AdobeOrg`す。
- **ホストとポート**:ホストエンドポイントと、そのクエリサービス用のポート。
- **データベース：** 使用するデータベースです。

>[!NOTE] ログイン資格情報、ホスト、ポートおよびデータベース名の検索について詳しくは、プラットフォームの資格情 [報ページを参照してください](https://platform.adobe.com/query/configuration)。 資格情報を検索するには、Platformにログインし、「 **クエリ**」をクリックし、「秘密鍵証明書」をクリ **ックします**。

![](../images/clients/aqua-data-studio/register-server-general-tab.png)

Select the **Driver** tab. [パラ *メータ*]で、値を `?sslmode=require`

![](../images/clients/aqua-data-studio/register-server-driver-tab.png)

接続の詳細を入力したら、「接続のテスト」を **クリックして** 、秘密鍵証明書が正しく機能することを確認します。 接続に成功した場合は、「保存」をクリ **ックし** 、サーバーを登録します。 登録が成功すると接続が *ダッシュボード* に表示され、サーバーに接続し、そのスキーマオブジェクトを表示できることを確認します。

## 次の手順

クエリサービスに接続したら、Aqua Data Studio内で *クエリアナライザ* (SQL)を実行および編集できます。 クエリの書き込みおよび実行方法の詳細については、『実行中のクエリ』ガイドを [参照してくださ](../creating-queries/creating-queries.md)い。