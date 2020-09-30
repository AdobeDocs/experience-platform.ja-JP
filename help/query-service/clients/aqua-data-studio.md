---
keywords: Experience Platform;home;popular topics;query service;Query service;Aqua Data Studio;Aqua data studio;connect to query service;
solution: Experience Platform
title: Aqua Data Studio との接続
topic: connect
description: このドキュメントでは、Aqua Data Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
translation-type: tm+mt
source-git-commit: 4b2df39b84b2874cbfda9ef2d68c4b50d00596ac
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 70%

---


# Connect with [!DNL Aqua Data Studio]

This document walks through the steps for connecting [!DNL Aqua Data Studio] with Adobe Experience Platform [!DNL Query Service].

After installing [!DNL Aqua Data Studio], you must first register the server. メインメニューで「**[!UICONTROL サーバー]**」をクリックし、「**[!UICONTROL サーバーを登録]**」をクリックします。

![](../images/clients/aqua-data-studio/register-server.png)

**[!UICONTROL サーバーを登録]**&#x200B;ダイアログが表示されます。「**[!UICONTROL 一般]**」タブで、左側のリストから「**[!UICONTROL PostgreSQL]**」を選択します。表示されるダイアログで、サーバー設定の次の詳細を指定します。

- **[!UICONTROL Name]**：接続の名前。
- **[!UICONTROL Login Name and Password]**：使用されるログイン資格情報。ユーザー名は、`ORG_ID@AdobeOrg` の形式をとります。
- **[!UICONTROL ホストとポート]**:のホストエンドポイントとそのポート [!DNL Query Service]。 に接続するには、ポート80を使用する必要があり [!DNL Query Service]ます。
- **[!UICONTROL データベース]:** 使用するデータベースです。

>[!NOTE]
>
> ログイン資格情報、ホスト、ポート、データベース名の検索について詳しくは、[Platform の資格情報ページ](https://platform.adobe.com/query/configuration)を参照してください。To find your credentials, log in to [!DNL Platform], click **[!UICONTROL Queries]**, then click **[!UICONTROL Credentials]**.

![](../images/clients/aqua-data-studio/register-server-general-tab.png)

「**[!UICONTROL ドライバー]**」タブを選択します。「**[!UICONTROL パラメーター]**」で、値を `?sslmode=require` に設定します。

![](../images/clients/aqua-data-studio/register-server-driver-tab.png)

接続の詳細を入力したら、「**[!UICONTROL 接続をテスト]**」をクリックして、資格情報が正しく機能することを確認します。接続に成功した場合は、「**[!UICONTROL 保存]**」をクリックして、サーバーを登録します。登録が成功すると接続が&#x200B;*ダッシュボード*&#x200B;に表示され、サーバーに接続してそのスキーマオブジェクトを表示できることを確認できます。

## 次の手順

Now that you have connected to [!DNL Query Service], you can use the **[!UICONTROL Query Analyzer]** within [!DNL Aqua Data Studio] to execute and edit SQL statements. クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../creating-queries/creating-queries.md)』を参照してください。