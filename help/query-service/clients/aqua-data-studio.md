---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Aqua Data Studioに接続
topic: connect
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# 接続先 [!DNL Aqua Data Studio]

このドキュメントでは、Adobe Experience Platformとの接続の手順 [!DNL Aqua Data Studio] について説明 [!DNL Query Service]します。

インストール後 [!DNL Aqua Data Studio]、まずサーバーを登録する必要があります。 メインメニューで「 **[!UICONTROL Server]**」をクリックし、「 **[!UICONTROL Register Server]**」をクリックします。

![](../images/clients/aqua-data-studio/register-server.png)

「 *[!UICONTROL Register Server]* 」ダイアログが表示されます。 「 *[!UICONTROL 一般]* 」タブで、左側のリストから **[!UICONTROL PostgreSQL]** を選択します。 表示されるダイアログで、サーバー設定の次の詳細を指定します。

- **[!UICONTROL 名前]**: 接続の名前。
- **[!UICONTROL Login Name and Password]**: 使用するログイン資格情報。 ユーザー名は、の形式をとり `ORG_ID@AdobeOrg`ます。
- **[!UICONTROL ホストとポート]**: のホストエンドポイントとそのポート [!DNL Query Service]。
- **[!UICONTROL データベース]:**使用するデータベースです。

>[!NOTE]
>
>ログイン資格情報、ホスト、ポートおよびデータベース名の検索について詳しくは、Platformの [資格情報ページを参照してください](https://platform.adobe.com/query/configuration)。 資格情報を探すには、にログインし、「 [!DNL Platform]クエリ **[!UICONTROL 」をクリックし、「]**&#x200B;資格情報 ****」をクリックします。

![](../images/clients/aqua-data-studio/register-server-general-tab.png)

Select the **[!UICONTROL Driver]** tab. [ *[!UICONTROL パラメータ]*]で、値を `?sslmode=require`

![](../images/clients/aqua-data-studio/register-server-driver-tab.png)

接続の詳細を入力したら、「接続を **[!UICONTROL テスト]** 」をクリックして、秘密鍵証明書が正しく動作することを確認します。 接続に成功した場合は、「 **[!UICONTROL 保存]** 」をクリックしてサーバーを登録します。 登録が成功すると *ダッシュボードに接続が表示され* 、サーバーに接続でき、スキーマオブジェクトの表示が完了したことを確認します。

## 次の手順

に接続したら、 [!DNL Query Service]クエリ・アナライザ *[!UICONTROL (]* ・アナライザ [!DNL Aqua Data Studio] )を使用して、SQL文を実行および編集できます。 クエリの書き込みおよび実行方法の詳細については、『 [実行中のクエリ』ガイドを参照してください](../creating-queries/creating-queries.md)。