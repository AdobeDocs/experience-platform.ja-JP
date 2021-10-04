---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure Event Hubs;Event Hubs;Azure Event Hubs;Azure Event Hubs
solution: Experience Platform
title: UI での Azure Event Hubs ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Azure Event Hubs ソース接続を作成する方法を説明します。
exl-id: 7e67e213-8ccb-4fa5-b09f-ae77aba8614c
source-git-commit: d6f1521470b8dc630060584189690545c724de6b
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 12%

---


# UI での [!DNL Azure Event Hubs] ソース接続の作成

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Azure Event Hubs]（以下「[!DNL Event Hubs]」と呼びます）ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Event Hubs] 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/streaming/cloud-storage-streaming.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL Event Hubs] ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `sasKeyName` | 承認規則の名前。SAS キー名とも呼ばれます。 |
| `sasKey` | 生成された共有アクセス署名。 |
| `namespace` | アクセスする [!DNL Event Hubs] の名前空間。 |

これらの値の詳細は、[ この  [!DNL Event Hubs]  ドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) を参照してください。

## [!DNL Event Hubs] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Event Hubs] アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 「**[!UICONTROL カタログ]**」タブには、アカウントを作成する際に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL クラウドストレージ]**」カテゴリで、「**[!UICONTROL Azure Event Hubs]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL Add data]**」を選択して、新しい Event Hubs コネクタを作成します。

![](../../../../images/tutorials/create/eventhub/catalog.png)

[**[!UICONTROL Azure Event Hubs に接続]**] ダイアログが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、[!DNL Event Hubs] 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/eventhub/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Event Hubs] アカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![](../../../../images/tutorials/create/eventhub/existing.png)

## 次の手順

このチュートリアルに従って、[!DNL Event Hubs] アカウントを [!DNL Platform] に接続しました。 次のチュートリアルに進み、クラウドストレージから  [!DNL Platform]](../../dataflow/streaming/cloud-storage-streaming.md) にデータを取り込むように [ データフローを設定します。
