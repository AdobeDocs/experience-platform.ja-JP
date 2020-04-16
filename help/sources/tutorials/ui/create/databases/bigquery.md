---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのGoogle Bigクエリソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでのGoogle Bigクエリソースコネクタの作成

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Platformユーザーインターフェイスを使用してGoogle Bigクエリ（以下「GBQ」と呼ばれる）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):カスタムスキーマを作成する方法についてスキーマエディターのUI。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

GBQベース接続が既にある場合は、このドキュメントの残りの部分をスキップし、データフローの設定に関するチュートリアルに進 [むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

プラットフォーム上のGBQアカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `project` | クエリ対象のデフォルトのBigQueryプロジェクトのプロジェクトID。 |
| `clientID` | 更新トークンの生成に使用されるID値。 |
| `clientSecret` | 更新トークンの生成に使用されるシークレット値。 |
| `refreshToken` | BigQueryへのアクセスを承認するために使用されるGoogleから取得された更新トークン。 |

これらの値の詳細は、このGBQドキュメント [](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing)。

## GBQアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、GBQアカウントをプラットフォームにリンクします。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアク *セスします* 。 カタ *ログ画面には* 、様々なソースが表示されます。このソースを使用して受信ベース接続を作成でき、各ソースに関連付けられた既存のベース接続の数が表示されます。

「 *Databases* 」カテゴリで「 **Google Bigクエリ** 」を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたはソースのドキュメントに接続するための表示が表示されます。 新しい受信ベース接続を作成するには、[ソースの接続] **を選択します**。

![](../../../../images/tutorials/create/google-big-query/sources-catalog.png)

「Google Big *クエリに接続* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「新規アカウント」 **を選択しま**&#x200B;す。 表示される入力フォームで、名前、オプションの説明、GBQ秘密鍵証明書を使用してベース接続を指定します。 終了したら、[接続 **** ]を選択し、新しいベース接続が確立されるまでの時間を設定します。

![](../../../../images/tutorials/create/google-big-query/gbq-new-credentials.png)

### 既存のアカウント

既存のアカウントを接続するには、接続するGBQアカウントを選択し、「次へ」を選 **択し** 、次に進みます。

![](../../../../images/tutorials/create/google-big-query/gbq-existing-credentials.png)

## 次の手順

このチュートリアルに従って、GBQアカウントへのベース接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに [取り込むようにデータフローを設定できます](../../dataflow/databases.md)。