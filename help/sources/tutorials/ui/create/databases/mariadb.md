---
keywords: Experience Platform；ホーム；人気のトピック；Maria DB;maria db
solution: Experience Platform
title: UI での MariaDB Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Maria DB ソース接続を作成する方法を説明します。
exl-id: 259ca112-01f1-414a-bf9f-d94caf4c69df
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 40%

---

# UI での [!DNL MariaDB] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して Maria DB ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL MariaDB] 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform] で [!DNL MariaDB] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | MariaDB 認証に関連付けられた接続文字列。 [!DNL MariaDB] の接続文字列パターンは `Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` です。 |

基本について詳しくは、この [[!DNL MariaDB]  ドキュメント ](https://mariadb.com/kb/en/about-mariadb-connector-odbc/) を参照してください。

## [!DNL Maria DB] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL Maria DB] アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

**[!UICONTROL Databases]** カテゴリの下で **[!UICONTROL Maria DB]** を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Maria DB] コネクタを作成します。

![](../../../../images/tutorials/create/maria-db/catalog.png)

**[!UICONTROL Maria DB に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL MariaDB] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![](../../../../images/tutorials/create/maria-db/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL MariaDB] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![](../../../../images/tutorials/create/maria-db/existing.png)

## 次の手順

このチュートリアルでは、[!DNL MariaDB] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/databases.md) を行いましょう。
