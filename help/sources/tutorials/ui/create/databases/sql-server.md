---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのMicrosoft SQL Serverソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 75ba0bce7ce070af851bbf7e220dbf08febc4c20
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---


# UIでのMicrosoft SQL Serverソースコネクタの作成

> [!NOTE]
> Microsoft SQL Serverコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してMicrosoft SQL Server（以下「SQL Server」と呼ばれる）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

SQL Serverベースの接続が既に存在する場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

プラットフォーム上のSQL Serverに接続するには、次の接続プロパティを指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | SQL Serverアカウントに関連付けられている接続文字列。 SQL Serverの接続文字列パターンは次のとおりです。 `Data Source={SERVER_NAME}\\<{INSTANCE_NAME} if using named instance>;Initial Catalog={DATABASE};Integrated Security=False;User ID={USERNAME};Password={PASSWORD};`. |

SQL Serverの使い始めについて詳しくは、 [このドキュメント](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server) を参照してください。

## SQL Serverアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、SQL Serverアカウントをプラットフォームにリンクできます。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

[ *Databases* ]カテゴリの下で[ **Microsoft SQL Server** ]を選択し、情報バーを画面の右側に表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **接続ソース**」を選択します。

![](../../../../images/tutorials/create/microsoft-sql-server/catalog.png)

「 *Connect to Microsoft SQL Server* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明、およびSQL Serverの資格情報を指定します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/microsoft-sql-server/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するSQL Serverアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![](../../../../images/tutorials/create/microsoft-sql-server/existing.png)

## 次の手順

このチュートリアルに従って、SQL Serverアカウントへの基本接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/databases.md)。