---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのMySQLソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 0a2247a9267d4da481b3f3a5dfddf45d49016e61
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 1%

---


# UIでのMySQLソースコネクタの作成

> [!NOTE]
> MySQL Connectorはベータ版です。 機能とドキュメントは、変更されることがあります。

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してMySQLソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

MySQLベースの接続が既にある場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

プラットフォームのMySQLアカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | アカウントに関連付けられているMySQL接続文字列。 MySQLの接続文字列パターンは次のとおりです。 `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |

接続文字列の詳細と、MySQLドキュメントを読むことによる接続文字列の取得方法について詳し [く説明します](https://dev.mysql.com/doc/connector-net/en/connector-net-connections-string.html)。

## MySQLアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、MySQLアカウントをプラットフォームにリンクできます。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *Databases* 」カテゴリの下で「 **MySQL** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **接続ソース**」を選択します。

![](../../../../images/tutorials/create/my-sql/sources-catalog.png)

「MySQL *に* 接続」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明、MySQL資格情報を指定します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/my-sql/new-credentials.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するMySQLアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![](../../../../images/tutorials/create/my-sql/existing-credentials.png)

## 次の手順

このチュートリアルに従って、MySQLアカウントへの基本接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/databases.md)。