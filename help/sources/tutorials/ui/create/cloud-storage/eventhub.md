---
title: UI での Azure Event Hubs ソース接続の作成
description: Adobe Experience Platform UI を使用して Azure Event Hubs ソース接続を作成する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 7e67e213-8ccb-4fa5-b09f-ae77aba8614c
source-git-commit: 1680cc4e1d5c1576767053a74e560bc2eb8c24cb
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 25%

---

# の作成 [!DNL Azure Event Hubs] UI のソース接続

>[!IMPORTANT]
>
>The [!DNL Azure Event Hubs] ソースは、Real-time Customer Data Platform Ultimate を購入したユーザーがソースカタログで利用できます。

このチュートリアルでは、 [!DNL Azure Event Hubs] Adobe Experience Platformユーザーインターフェイスを使用するアカウント。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Event Hubs] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/streaming/cloud-storage-streaming.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

を認証するために、 [!DNL Event Hubs] ソースコネクタの場合、次の接続プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB 標準認証]

| 資格情報 | 説明 |
| --- | --- |
| SAS キー名 | 認証規則の名前。SAS キー名とも呼ばれます。 |
| SAS キー | のプライマリキー [!DNL Event Hubs] 名前空間。 The `sasPolicy` この `sasKey` 必ず～に対応する `manage` 次に対して設定された権限： [!DNL Event Hubs] リストに値を入力します。 |
| 名前空間 | の名前空間 [!DNL Event Hubs] にアクセスしています。 An [!DNL Event Hubs] 名前空間は、1 つ以上のスコーピングコンテナを作成できる一意のスコーピングコンテナを提供します。 [!DNL Event Hubs]. |

>[!TAB SAS 認証]

| 資格情報 | 説明 |
| --- | --- |
| SAS キー名 | 認証規則の名前。SAS キー名とも呼ばれます。 |
| SAS キー | のプライマリキー [!DNL Event Hubs] 名前空間。 The `sasPolicy` この `sasKey` 必ず～に対応する `manage` 次に対して設定された権限： [!DNL Event Hubs] リストに値を入力します。 |
| 名前空間 | の名前空間 [!DNL Event Hubs] にアクセスしています。 An [!DNL Event Hubs] 名前空間は、1 つ以上のスコーピングコンテナを作成できる一意のスコーピングコンテナを提供します。 [!DNL Event Hubs]. |
| イベントハブ名 | の名前 [!DNL Event Hubs] ソース。 |

>[!ENDTABS]

これらの値について詳しくは、 [この Event Hubs ドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).

必要な資格情報を収集したら、次の手順に従って、 [!DNL Event Hubs] アカウントからExperience Platformへ。

## [!DNL Event Hubs] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。The [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL Azure イベントハブ]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![Azure Event Hubs が選択されたソースカタログ。](../../../../images/tutorials/create/eventhub/catalog.png)

The **[!UICONTROL Azure Event Hubs に接続]** ダイアログが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、 [!DNL Event Hubs] 使用するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存の Azure Event Hubs ソースアカウントの一覧です。](../../../../images/tutorials/create/eventhub/existing.png)

### 新規アカウント

>[!TIP]
>
>作成後は、 [!DNL Event Hubs] ベース接続。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

新しいアカウントを作成するには、 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、新しい [!DNL Event Hubs] アカウント。

![Azure Event Hubs 用の新しいアカウント作成インターフェイス。](../../../../images/tutorials/create/eventhub/new.png)

>[!BEGINTABS]

>[!TAB 標準認証]

次の手順で、 [!DNL Event Hubs] 標準認証のアカウント： **[!UICONTROL 標準認証]** 次に、 [!UICONTROL SAS キー名], [!UICONTROL SAS キー]、および [!UICONTROL 名前空間].

認証資格情報を入力したら、「 」を選択します。 **[!UICONTROL ソースに接続]**.

![Azure Event Hubs の標準認証インターフェイス。](../../../../images/tutorials/create/eventhub/standard.png)

>[!TAB SAS 認証]

次の手順で、 [!DNL Event Hubs] SAS 認証を使用するアカウント、 **[!UICONTROL SAS 認証]** 次に、 [!UICONTROL SAS キー名], [!UICONTROL SAS キー], [!UICONTROL 名前空間]、および [!UICONTROL イベントハブ名].

認証資格情報を入力したら、「 」を選択します。 **[!UICONTROL ソースに接続]**.

![Azure Event Hubs 用の SAS 認証インターフェイス。](../../../../images/tutorials/create/eventhub/sas.png)

>[!ENDTABS]


## 次の手順

このチュートリアルに従うことで、 [!DNL Event Hubs] アカウントからExperience Platformへ。 次のチュートリアルに進み、 [データフローを設定して、クラウドストレージからExperience Platformにデータを取り込みます。](../../dataflow/streaming/cloud-storage-streaming.md).
