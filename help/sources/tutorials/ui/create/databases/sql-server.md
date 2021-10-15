---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft SQL Server;SQL Server;SQL Server
solution: Experience Platform
title: UI でのMicrosoft SQL Server ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してMicrosoft SQL Server ソース接続を作成する方法を説明します。
exl-id: aba4e317-1c59-4999-a525-dba15f8d4df9
source-git-commit: b2384bfe26fa3d111c342062b2d9bb37c4226857
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 12%

---

# UI での [!DNL Microsoft SQL Server] ソース接続の作成

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Microsoft SQL Server]（以下「[!DNL SQL Server]」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL SQL Server] 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL Platform] の [!DNL SQL Server] に接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL SQL Server] アカウントに関連付けられた接続文字列。 [!DNL SQL Server] 接続文字列パターンは次のとおりです。`Data Source={SERVER_NAME}\\<{INSTANCE_NAME} if using named instance>;Initial Catalog={DATABASE};Integrated Security=False;User ID={USERNAME};Password={PASSWORD};`. |

使い始める方法については、[ この  [!DNL SQL Server]  ドキュメント ](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server) を参照してください。

## [!DNL SQL Server] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って [!DNL SQL Server] アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL Databases]**」カテゴリで、「**[!UICONTROL Microsoft SQL Server]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して新しい [!DNL SQL Server] コネクタを作成します。

![](../../../../images/tutorials/create/microsoft-sql-server/catalog.png)

「**[!UICONTROL Microsoft SQL Server に接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、[!DNL SQL Server] 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/microsoft-sql-server/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL SQL Server] アカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![](../../../../images/tutorials/create/microsoft-sql-server/existing.png)

## 次の手順

このチュートリアルに従って、[!DNL SQL Server] アカウントへの接続を確立しました。 次のチュートリアルに進み、[ にデータを取り込むようにデータフローを設定します。 [!DNL Platform]](../../dataflow/databases.md)
