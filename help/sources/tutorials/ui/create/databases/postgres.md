---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでPostgreSQLソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 5ad763d2167c68f3293a2813248efaee22230a52
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 1%

---


# UIでPostgreSQLソースコネクタを作成する

> [!NOTE]
> PostgreSQLコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Platformのユーザインターフェイスを使用してPostgreSQL （以下「PSQL」と呼ばれます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

PSQLベースの接続が既に存在する場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

Platform上のPSQLアカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | PSQLアカウントに関連付けられている接続文字列です。 PSQL接続文字列パターンは次のとおりです。 `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}`. |

使用を開始する方法の詳細については、この [PSQLドキュメントを参照してください](https://www.postgresql.org/docs/9.2/app-psql.html)。

## PSQLアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、PSQLアカウントをPlatformにリンクできます。

「 <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platform</a> 」にログインし、左のナビゲーションバーで「 **ソース** 」を選択して「 *ソース* 」ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *Databases* 」カテゴリの下で、「 **PostgreSQL DB** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、**「Connect source**」を選択します。

![](../../../../images/tutorials/create/psql/catalog.png)

「 *PSQLに* 接続」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明、およびPSQL秘密鍵証明書を指定します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/psql/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するPSQLアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![](../../../../images/tutorials/create/psql/existing.png)

## 次の手順

このチュートリアルに従って、PSQLアカウントへの基本接続を確立しました。 次のチュートリアルに進み、データをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/databases.md)。