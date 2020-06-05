---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでGoogle Bigクエリソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 75ba0bce7ce070af851bbf7e220dbf08febc4c20
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---


# UIでGoogle Bigクエリソースコネクタを作成する

> [!NOTE]
> Google BigQueryコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してGoogle Bigクエリ（以下「GBQ」と呼ばれる）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

GBQベース接続が既にある場合は、このドキュメントの残りの部分をスキップして、データ・フローの [構成に関するチュートリアルに進みます](../../dataflow/databases.md)。

### 必要な資格情報の収集

プラットフォーム上のGBQアカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `project` | クエリするデフォルトのBigQueryプロジェクトのプロジェクトID。 |
| `clientID` | 更新トークンの生成に使用されるID値。 |
| `clientSecret` | 更新トークンの生成に使用されるシークレット値。 |
| `refreshToken` | BigQueryへのアクセスを許可するために使用される、Googleから取得される更新トークンです。 |

これらの値の詳細は、 [このGBQドキュメントを参照してください](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing)。

## GBQアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、GBQアカウントをプラットフォームにリンクします。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *Databases* 」カテゴリの下で「 **Google Bigクエリ** 」を選択し、情報バーを画面の右側に表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **接続ソース**」を選択します。

![](../../../../images/tutorials/create/google-big-query/catalog.png)

「Google Bigクエリ *に接続* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、名前、オプションの説明、GBQ秘密鍵証明書を持つ基本接続を指定します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/google-big-query/new.png)

### 既存のアカウント

既存のアカウントを接続するには、接続するGBQアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![](../../../../images/tutorials/create/google-big-query/existing.png)

## 次の手順

このチュートリアルに従って、GBQアカウントへの基本接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/databases.md)。