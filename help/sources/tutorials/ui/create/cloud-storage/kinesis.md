---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのAmazon Kinesisソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 1eb6883ec9b78e5d4398bb762bba05a61c0f8308
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 1%

---


# UIでのAmazon Kinesisソースコネクタの作成

>[!NOTE]
> Amazon Kinesisコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してAmazon Kinesis（以下「Kinesis」と呼ばれる）ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にKinesisアカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/streaming/cloud-storage.md)。

### 必要な資格情報の収集

Kinesisソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `accessKeyId` | KinesisアカウントのアクセスキーID。 |
| `Secret access key` | Kinesisアカウントの秘密アクセスキー。 |
| `region` | AWSサーバーの地域です。 |

これらの値の詳細については、 [このKinesisドキュメントを参照してください](https://docs.aws.amazon.com/streams/latest/dev/getting-started.html)。

## Kinesisアカウントの接続

必要な資格情報を収集したら、次の手順に従ってKinesisアカウントをプラットフォームにリンクできます。

[Adobe Experience Platformにログインし、左のナビゲーションバーで「](https://platform.adobe.com) Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 「 *カタログ* 」タブには、プラットフォームに接続できる様々なソースが表示されます。 各ソースには、関連付けられた既存のアカウントの数が表示されます。

「 *クラウドストレージ***」カテゴリで、「** Amazon Kinesis」を選択し、「+」アイコン(+) **** をクリックして新しいKinesisコネクタを作成します。

![](../../../../images/tutorials/create/eventhub/catalog.png)

[ *Amazon Kinesisに* 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームに、名前、オプションの説明、Kinesis資格情報を入力します。 終了したら、 **[接続** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/eventhub/new.png)

### 既存のアカウント

既存のアカウントを接続するには、接続するKinesisアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![](../../../../images/tutorials/create/eventhub/existing.png)

## 次の手順

このチュートリアルに従うことで、Kinesisアカウントをプラットフォームに接続できました。 次のチュートリアルに進み、クラウドストレージのデータをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/streaming/cloud-storage.md)。