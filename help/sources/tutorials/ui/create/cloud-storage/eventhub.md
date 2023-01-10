---
keywords: Experience Platform；ホーム；人気の高いトピック；Azure イベントハブ；イベントハブ；Azure イベントハブ
solution: Experience Platform
title: UI での Azure Event Hubs ソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Azure Event Hubs ソース接続を作成する方法を説明します。
exl-id: 7e67e213-8ccb-4fa5-b09f-ae77aba8614c
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 39%

---


# の作成 [!DNL Azure Event Hubs] UI のソース接続

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Azure Event Hubs] （以下「」という。）[!DNL Event Hubs]&quot;) を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Event Hubs] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/streaming/cloud-storage-streaming.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

を認証するために [!DNL Event Hubs] ソースコネクタの場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `sasKeyName` | 認証規則の名前。SAS キー名とも呼ばれます。 |
| `sasKey` | のプライマリキー [!DNL Event Hubs] 名前空間。 この `sasPolicy` この `sasKey` 必ず～に対応する `manage` 次に対して設定された権限： [!DNL Event Hubs] リストに値を入力します。 |
| `namespace` | の名前空間 [!DNL Event Hubs] にアクセスしています。 |

これらの値について詳しくは、 [この [!DNL Event Hubs] 文書](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).

## [!DNL Event Hubs] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Event Hubs] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 この **[!UICONTROL カタログ]** 「 」タブには、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL クラウドストレージ]** カテゴリ、選択 **[!UICONTROL Azure イベントハブ]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい Event Hubs コネクタを作成するには、以下を実行します。

![](../../../../images/tutorials/create/eventhub/catalog.png)

この **[!UICONTROL Azure イベントハブに接続]** ダイアログが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新規アカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL Event Hubs] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/eventhub/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Event Hubs] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![](../../../../images/tutorials/create/eventhub/existing.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Event Hubs] アカウント [!DNL Platform]. 次のチュートリアルに進み、 [データフローを設定して、クラウドストレージからにデータを取り込みます。 [!DNL Platform]](../../dataflow/streaming/cloud-storage-streaming.md).
