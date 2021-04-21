---
keywords: Experience Platform；ホーム；人気の高いトピック；Microsoft SQL Server;SQL Server;sql server
solution: Experience Platform
title: UIでのMicrosoft SQL Serverソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してMicrosoft SQL Serverソース接続を作成する方法を説明します。
exl-id: aba4e317-1c59-4999-a525-dba15f8d4df9
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 9%

---

# UIに[!DNL Microsoft SQL Server]ソース接続を作成する

>[!NOTE]
>
> [!DNL Microsoft SQL Server]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Microsoft SQL Server] （以下「[!DNL SQL Server]」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL SQL Server]接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform]の[!DNL SQL Server]に接続するには、次の接続プロパティを指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL SQL Server]アカウントに関連付けられている接続文字列。 [!DNL SQL Server]接続文字列パターンは次のとおりです。`Data Source={SERVER_NAME}\\<{INSTANCE_NAME} if using named instance>;Initial Catalog={DATABASE};Integrated Security=False;User ID={USERNAME};Password={PASSWORD};`. |

開始方法の詳細については、[この [!DNL SQL Server] ドキュメント](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server)を参照してください。

## [!DNL SQL Server]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL SQL Server]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL Databases]**&#x200B;カテゴリの下で、**[!UICONTROL Microsoft SQL Server]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL SQL Server]コネクタを作成します。

![](../../../../images/tutorials/create/microsoft-sql-server/catalog.png)

[**[!UICONTROL Microsoft SQL Server]**&#x200B;に接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL SQL Server]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/microsoft-sql-server/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL SQL Server]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![](../../../../images/tutorials/create/microsoft-sql-server/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL SQL Server]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/databases.md)に取り込むようにデータフローを設定できます。
