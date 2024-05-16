---
title: UI でのMicrosoft SQL Server ソース接続の作成
description: Adobe Experience Platform UI を使用してMicrosoft SQL Server ソース接続を作成する方法を説明します。
exl-id: aba4e317-1c59-4999-a525-dba15f8d4df9
source-git-commit: 1828dd76e9ff317f97e9651331df3e49e44efff5
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 45%

---

# UI での [!DNL Microsoft SQL Server] ソース接続の作成

を接続する方法については、このチュートリアルをお読みください。 [!DNL Microsoft SQL Server] ユーザーインターフェイスを使用してAdobe Experience Platformにアカウント設定します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL SQL Server] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

に接続するには [!DNL SQL Server] 日付： [!DNL Platform]には、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| 接続文字列 | に関連付けられた接続文字列 [!DNL Microsoft SQL Server] アカウント。 接続文字列のパターンは、データソースにサーバー名とインスタンス名のどちらを使用しているかによって異なります。<ul><li>サーバー名を使用した接続文字列： `Data Source={SERVER_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};`</li><li>インスタンス名を使用した接続文字列：`Data Source={INSTANCE_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};` | `Data Source=mssqlserver.database.windows.net;Initial Catalog=mssqlserver_e2e_db;Integrated Security=False;User ID=mssqluser;Password=mssqlpassword` |

基本について詳しくは、を参照してください。 [この [!DNL SQL Server] 文書](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server).

## [!DNL SQL Server] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 *データベース* カテゴリ、選択 **[!DNL Microsoft SQL Server]**&#x200B;を選択してから、 **[!UICONTROL の設定]**.

>[!TIP]
>
>ソースカタログ内のソースには、 **[!UICONTROL の設定]** 特定のソースがまだ認証済みアカウントを持っていない場合のオプション。 認証済みアカウントが存在すると、このオプションはに変更されます。 **[!UICONTROL データを追加]**.

![Microsoft SQL Server ソースが選択されたソースカタログ](../../../../images/tutorials/create/microsoft-sql-server/catalog.png)

この **[!UICONTROL Microsoft SQL Server への接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

>[!BEGINTABS]

>[!TAB 新しいアカウントを作成]

新しいアカウントを作成するには、を選択します **[!UICONTROL 新しいアカウント]** に加えて、名前、説明（オプション）、の認証情報を指定します。

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ソース接続の詳細が入力され、ハイライト表示された新しいアカウントインターフェイス。](../../../../images/tutorials/create/microsoft-sql-server/new.png)

>[!TAB 既存のアカウントを使用]

既存のアカウントを使用するには、を選択します **[!UICONTROL 既存のアカウント]** 次に、既存のアカウントカタログから使用するアカウントを選択します。

「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存のアカウントのリストを表示する既存のアカウントインターフェイス。](../../../../images/tutorials/create/microsoft-sql-server/existing.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL SQL Server] アカウントとの接続を確立しました。次のチュートリアルに進むことができます。 [データをに取り込むデータフローの設定 [!DNL Platform]](../../dataflow/databases.md).
