---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIにAzureイベントハブのソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 16%

---


# Create an [!DNL Azure Event Hubs] source connector in the UI

>[!NOTE]
> コネクタ [!DNL Azure Event Hubs] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Azure Event Hubs] ユーザインターフェイスを使用して、[!DNL Event Hubs](以下「 [!DNL Platform] 」と呼ばれる)ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に [!DNL Event Hubs] アカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/streaming/cloud-storage.md)。

### 必要な資格情報の収集

ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があり [!DNL Event Hubs] ます。

| Credential | 説明 |
| ---------- | ----------- |
| `sasKeyName` | 認証規則の名前。SASキー名とも呼ばれます。 |
| `sasKey` | 生成された共有アクセス署名です。 |
| `namespace` | アクセスしているユーザの名前空間 [!DNL Event Hubs] です。 |

これらの値の詳細については、 [次の「イベントハブのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)」を参照してください。

## アカウントに接続 [!DNL Event Hubs] する

必要な資格情報を収集したら、次の手順に従って [!DNL Event Hubs] アカウントをにリンクでき [!DNL Platform]ます。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *ソース* 」ワークスペースにアクセスします。 「 *[!UICONTROL カタログ]* 」タブには、接続可能な様々なソースが表示され [!DNL Platform]ます。 各ソースには、関連付けられた既存のアカウントの数が表示されます。

[ *[!UICONTROL Cloudストレージ]***カテゴリで、[]** Azureイベントハブ]を選択し、[+]アイコン(+) **** をクリックして新しいイベントハブコネクタを作成します。

![](../../../../images/tutorials/create/eventhub/catalog.png)

[ *[!UICONTROL Azureイベントハブに]* 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、イベントハブの資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/eventhub/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するイベントハブアカウントを選択し、[ **[!UICONTROL 次へ]** ]を選択して次に進みます。

![](../../../../images/tutorials/create/eventhub/existing.png)

## 次の手順

このチュートリアルに従って、イベントハブアカウントをに接続し [!DNL Platform]ました。 次のチュートリアルに進み、クラウドストレージのデータをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/streaming/cloud-storage.md)。