---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；Aqua Data Studio;Aqua data studio；クエリサービスへの接続；
solution: Experience Platform
title: Aqua Data Studioをクエリサービスに接続
topic-legacy: connect
description: このドキュメントでは、Aqua Data Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 4770e221-48a7-45d8-80a4-60b5cbc0ec33
source-git-commit: 910a38ccb556ec427584d9b522e29f6877d1c987
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 23%

---

# [!DNL Aqua Data Studio]をクエリサービスに接続

このドキュメントでは、[!DNL Aqua Data Studio]をAdobe Experience Platform [!DNL Query Service]に接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL Aqua Data Studio]へのアクセス権を既に持っており、インターフェイスの操作方法に精通していることを前提としています。 [!DNL Aqua Data Studio]に関する詳細は、[公式の [!DNL Aqua Data Studio] ドキュメント](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation21.1/page/0/Aqua-Data-Studio-21-1)を参照してください。

[!DNL Aqua Data Studio]をインストールした後、まずサーバを登録する必要があります。 メインメニューから&#x200B;**[!DNL Server]**&#x200B;を選択し、その後に&#x200B;**[!DNL Register Server]**&#x200B;を選択します。

![](../images/clients/aqua-data-studio/register-server.png)

**[!DNL Register Server]**&#x200B;ダイアログが表示されます。 「**[!DNL General]**」タブで、左側のリストから「**[!DNL PostgreSQL]**」を選択します。 表示されるダイアログで、サーバー設定の次の詳細を指定します。

- **[!DNL Name]**：接続の名前。
- **[!DNL Login Name and Password]**:使用するログイン資格情報。ユーザー名は、`ORG_ID@AdobeOrg` の形式をとります。
- **[!DNL Host and Port]**:ホストエンドポイントと、そののポー [!DNL Query Service]ト。[!DNL Query Service]に接続するには、ポート80を使用する必要があります。
- **[!DNL Database]**：使用するデータベース。

>[!NOTE]
>
>ログイン資格情報、ホスト、ポート、データベース名の検索について詳しくは、『[資格情報ガイド](../ui/credentials.md)』を参照してください。 資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;を選択し、**[!UICONTROL 資格情報]**&#x200B;を選択します。

![](../images/clients/aqua-data-studio/register-server-general-tab.png)

「**[!DNL Driver]**」タブを選択します。**[!DNL Parameters]**&#x200B;の下で、値を`?sslmode=require`に設定します。

![](../images/clients/aqua-data-studio/register-server-driver-tab.png)

接続の詳細を入力したら、**[!DNL Test Connection]**&#x200B;を選択し、資格情報が正しく機能することを確認します。 接続に成功した場合は、**[!DNL Save]**&#x200B;を選択してサーバーを登録します。 登録が成功すると接続がダッシュボードに表示され、サーバーに接続してそのスキーマオブジェクトを表示できることを確認できます。

## 次の手順

[!DNL Query Service]に接続したら、[!DNL Aqua Data Studio]内で&#x200B;**[!DNL Query Analyzer]**&#x200B;を使用してSQL文を実行および編集できます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
