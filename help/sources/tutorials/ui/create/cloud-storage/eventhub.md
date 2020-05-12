---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIにAzureイベントハブのソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 1eb6883ec9b78e5d4398bb762bba05a61c0f8308
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 1%

---


# UIにAzureイベントハブのソースコネクタを作成する

>[!NOTE]
> Azureイベントハブコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Azureイベントハブ（以下「EventHub」と呼ばれる）ソースコネクタをプラットフォームユーザーインターフェイスを使用して認証する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にEventHubアカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/streaming/cloud-storage.md)。

### 必要な資格情報の収集

EventHubソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `sasKeyName` | 認証規則の名前。SASキー名とも呼ばれます。 |
| `sasKey` | 生成された共有アクセス署名です。 |
| `namespace` | アクセスするEventHubの名前空間。 |

これらの値の詳細については、 [このEventHubドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)。

## EventHubアカウントの接続

必要な資格情報を収集したら、次の手順に従ってEventHubアカウントをプラットフォームにリンクできます。

[Adobe Experience Platformにログインし、左のナビゲーションバーで「](https://platform.adobe.com) Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 「 *カタログ* 」タブには、プラットフォームに接続できる様々なソースが表示されます。 各ソースには、関連付けられた既存のアカウントの数が表示されます。

「 *Cloudストレージ* 」カテゴリで、「 **Azureイベントハブ」を選択し** 、「+」アイコン(+) **** をクリックして新しいEventHubコネクタを作成します。

![](../../../../images/tutorials/create/eventhub/catalog.png)

[ *Azureイベントハブに* 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、名前、オプションの説明およびEventHub資格情報を入力します。 終了したら、 **[接続** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/eventhub/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するEventHubアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![](../../../../images/tutorials/create/eventhub/existing.png)

## 次の手順

このチュートリアルに従って、EventHubアカウントをプラットフォームに接続しました。 次のチュートリアルに進み、クラウドストレージのデータをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/streaming/cloud-storage.md)。