---
title: UI でのMicrosoft SQL Server Source接続の作成
description: Adobe Experience Platform UI を使用してMicrosoft SQL Server ソース接続を作成する方法を説明します。
exl-id: aba4e317-1c59-4999-a525-dba15f8d4df9
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 42%

---

# UI での [!DNL Microsoft SQL Server] ソース接続の作成

ユーザーインターフェイスを使用して [!DNL Microsoft SQL Server] アカウントをAdobe Experience Platformに接続する方法については、このチュートリアルをお読みください。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL SQL Server] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Experience Platform] で [!DNL SQL Server] に接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| 接続文字列 | [!DNL Microsoft SQL Server] アカウントに関連付けられた接続文字列。 接続文字列のパターンは、データソースにサーバー名とインスタンス名のどちらを使用しているかによって異なります。<ul><li>サーバー名 `Data Source={SERVER_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};` を使用した接続文字列</li><li>インスタンス名を使用した接続文字列：`Data Source={INSTANCE_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};` | `Data Source=mssqlserver.database.windows.net;Initial Catalog=mssqlserver_e2e_db;Integrated Security=False;User ID=mssqluser;Password=mssqlpassword` |

基本について詳しくは、[&#x200B; このドキュメント  [!DNL SQL Server]  を参照してください &#x200B;](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server)。

## [!DNL SQL Server] アカウントを接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*Databases* カテゴリの下の [**[!DNL Microsoft SQL Server]**] を選択し、[**[!UICONTROL 設定]**] を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントが存在すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![&#x200B; ソースカタログとMicrosoft SQL Server ソースが選択されています。](../../../../images/tutorials/create/microsoft-sql-server/catalog.png)

**[!UICONTROL Microsoft SQL Server に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

>[!BEGINTABS]

>[!TAB  新規アカウントの作成 ]

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、名前、説明（オプション）、の資格情報を入力します。

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![&#x200B; ソース接続の詳細が入力され、ハイライト表示された新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/microsoft-sql-server/new.png)

>[!TAB  既存のアカウントを使用 ]

既存のアカウントを使用するには、「**[!UICONTROL 既存のアカウント]**」を選択し、既存のアカウントカタログから使用するアカウントを選択します。

「**[!UICONTROL 次へ]**」を選択して次に進みます。

![&#x200B; 既存のアカウントのリストを表示する既存のアカウントインターフェイス。](../../../../images/tutorials/create/microsoft-sql-server/existing.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL SQL Server] アカウントとの接続を確立しました。次のチュートリアルに進み、[&#x200B; データをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/databases.md) を行いましょう。
