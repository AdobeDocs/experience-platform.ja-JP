---
keywords: Experience Platform;home;popular topics;query service;Query service;Aqua Data Studio;Aqua data studio;connect to query service;
solution: Experience Platform
title: Aqua Data Studio との接続
topic: connect
description: このドキュメントでは、Aqua Data Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
translation-type: tm+mt
source-git-commit: 9fbb6b829cd9ddec30f22b0de66874be7710e465
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 70%

---


# [!DNL Aqua Data Studio]に接続

このドキュメントでは、[!DNL Aqua Data Studio]とAdobe Experience Platform[!DNL Query Service]を結ぶ手順を順を追って説明します。

[!DNL Aqua Data Studio]をインストールした後は、まずサーバを登録する必要があります。 メインメニューで「**[!UICONTROL サーバー]**」をクリックし、「**[!UICONTROL サーバーを登録]**」をクリックします。

![](../images/clients/aqua-data-studio/register-server.png)

**[!UICONTROL サーバーを登録]**&#x200B;ダイアログが表示されます。「**[!UICONTROL 一般]**」タブで、左側のリストから「**[!UICONTROL PostgreSQL]**」を選択します。表示されるダイアログで、サーバー設定の次の詳細を指定します。

- **[!UICONTROL Name]**：接続の名前。
- **[!UICONTROL Login Name and Password]**：使用されるログイン資格情報。ユーザー名は、`ORG_ID@AdobeOrg` の形式をとります。
- **[!UICONTROL ホストとポート]**:のホストエンドポイントとそのポート [!DNL Query Service]。[!DNL Query Service]に接続するには、ポート80を使用する必要があります。
- **[!UICONTROL Database]:** 使用するデータベース。

>[!NOTE]
>
> ログイン資格情報、ホスト、ポート、データベース名の検索について詳しくは、[Platform の資格情報ページ](https://platform.adobe.com/query/configuration)を参照してください。資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;をクリックしてから、**[!UICONTROL 資格情報]**&#x200B;をクリックします。

![](../images/clients/aqua-data-studio/register-server-general-tab.png)

「**[!UICONTROL ドライバー]**」タブを選択します。「**[!UICONTROL パラメーター]**」で、値を `?sslmode=require` に設定します。

![](../images/clients/aqua-data-studio/register-server-driver-tab.png)

接続の詳細を入力したら、「**[!UICONTROL 接続をテスト]**」をクリックして、資格情報が正しく機能することを確認します。接続に成功した場合は、「**[!UICONTROL 保存]**」をクリックして、サーバーを登録します。登録が成功すると接続が&#x200B;**ダッシュボード**&#x200B;に表示され、サーバーに接続してそのスキーマオブジェクトを表示できることを確認できます。

## 次の手順

[!DNL Query Service]に接続したら、[!DNL Aqua Data Studio]内で&#x200B;**[!UICONTROL クエリアナライザ]**&#x200B;を使用してSQL文を実行し、編集できます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。