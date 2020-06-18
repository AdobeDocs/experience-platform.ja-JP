---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIにAzureイベントハブのソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 855f543a1cef394d121502f03471a60b97eae256
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 1%

---


# UIにAzureイベントハブのソースコネクタを作成する

>[!NOTE]
> Azureイベントハブコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Platformユーザーインターフェイスを使用してAzureイベントハブ(以下「イベントハブ」と呼ばれる)ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にイベントハブのアカウントをお持ちの場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/streaming/cloud-storage.md)。

### 必要な資格情報の収集

イベントハブのソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `sasKeyName` | 認証規則の名前。SASキー名とも呼ばれます。 |
| `sasKey` | 生成された共有アクセス署名です。 |
| `namespace` | アクセスしているイベントハブの名前空間。 |

これらの値の詳細については、 [次の「イベントハブのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)」を参照してください。

## イベントハブアカウントの接続

必要な資格情報を収集したら、次の手順に従って、イベントハブのアカウントをPlatformにリンクできます。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **ソース** 」を選択して「 *ソース* 」ワークスペースにアクセスします。 「 *カタログ* 」タブには、Platformに接続できる様々なソースが表示されます。 各ソースには、関連付けられた既存のアカウントの数が表示されます。

[ *Cloudストレージ***]カテゴリで、[** Azureイベントハブ]を選択し、[+]アイコン(+) **** をクリックして新しいイベントハブコネクタを作成します。

![](../../../../images/tutorials/create/eventhub/catalog.png)

[ *Azureイベントハブに* 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、名前、オプションの説明、イベントハブの資格情報を入力します。 終了したら、 **[接続** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/eventhub/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するイベントハブアカウントを選択し、[ **次へ** ]を選択して次に進みます。

![](../../../../images/tutorials/create/eventhub/existing.png)

## 次の手順

このチュートリアルに従って、イベントハブのアカウントをPlatformに接続しました。 次のチュートリアルに進み、クラウドストレージのデータをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/streaming/cloud-storage.md)。