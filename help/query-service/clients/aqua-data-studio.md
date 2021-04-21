---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；Aqua Data Studio;Aqua data studio;クエリサービスに接続；
solution: Experience Platform
title: Aqua Data Studioをクエリサービスに接続
topic-legacy: connect
description: このドキュメントでは、Aqua Data Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 4770e221-48a7-45d8-80a4-60b5cbc0ec33
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 30%

---

# [!DNL Aqua Data Studio]をクエリサービスに接続

このドキュメントでは、[!DNL Aqua Data Studio]をAdobe Experience Platform[!DNL Query Service]に接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL Aqua Data Studio]へのアクセス権が既にあり、そのインターフェイスの操作方法に精通していることを前提としています。 [!DNL Aqua Data Studio]に関する詳細は、[正式な [!DNL Aqua Data Studio] ドキュメント](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation21.1/page/0/Aqua-Data-Studio-21-1)を参照してください。

[!DNL Aqua Data Studio]をインストールした後は、まずサーバを登録する必要があります。 メインメニューで&#x200B;**[!DNL Server]**&#x200B;を選択し、次に&#x200B;**[!DNL Register Server]**&#x200B;を選択します。

![](../images/clients/aqua-data-studio/register-server.png)

**[!DNL Register Server]**&#x200B;ダイアログが表示されます。 「**[!DNL General]**」タブで、左側のリストから「**[!DNL PostgreSQL]**」を選択します。 表示されるダイアログで、サーバー設定の次の詳細を指定します。

- **[!DNL Name]**：接続の名前。
- **[!DNL Login Name and Password]**:使用するログイン資格情報。ユーザー名は、`ORG_ID@AdobeOrg` の形式をとります。
- **[!DNL Host and Port]**:のホストエンドポイントとそのポート [!DNL Query Service]。[!DNL Query Service]に接続するには、ポート80を使用する必要があります。
- **[!DNL Database]**：使用するデータベース。

>[!NOTE]
>
> ログイン資格情報、ホスト、ポート、データベース名の検索について詳しくは、[Platform の資格情報ページ](https://platform.adobe.com/query/configuration)を参照してください。資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;を選択してから、**[!UICONTROL 資格情報]**&#x200B;を選択します。

![](../images/clients/aqua-data-studio/register-server-general-tab.png)

「**[!DNL Driver]**」タブを選択します。**[!DNL Parameters]**&#x200B;の下で、値を`?sslmode=require`に設定します

![](../images/clients/aqua-data-studio/register-server-driver-tab.png)

接続の詳細を入力したら、**[!DNL Test Connection]**&#x200B;を選択して、秘密鍵証明書が正しく機能することを確認します。 接続に成功した場合は、**[!DNL Save]**&#x200B;を選択してサーバーを登録します。 登録が成功するとダッシュボードに接続が表示され、サーバーに接続してスキーマオブジェクトを表示できることが確認されます。

## 次の手順

[!DNL Query Service]に接続したので、[!DNL Aqua Data Studio]内で&#x200B;**[!DNL Query Analyzer]**&#x200B;を使用してSQL文を実行し、編集できます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
