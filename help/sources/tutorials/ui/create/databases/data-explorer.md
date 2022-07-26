---
keywords: Experience Platform；ホーム；人気の高いトピック；AzureData Explorer;Azure Data Explorer;Data Explorer;Data Explorer
solution: Experience Platform
title: UI での AzureData Explorerソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して AzureData Explorerソース接続を作成する方法を説明します。
exl-id: 561bf948-fc92-4401-8631-e2a408667507
source-git-commit: 1e2644b7d83a0bcb7175f27d7c4859c0efba4060
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 48%

---

# の作成 [!DNL Azure Data Explorer] UI のソース接続

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Azure Data Explorer] （以下「」という。）[!DNL Data Explorer]&quot;) を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Data Explorer] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

 で [!DNL Data Explorer] アカウントにアクセスするには、次の値を指定する必要があります。[!DNL Platform]

| 認証情報 | 説明 |
| ---------- | ----------- |
| `endpoint` | のエンドポイント [!DNL Data Explorer] サーバー。 |
| `database` | の名前 [!DNL Data Explorer] データベース。 |
| `tenant` | への接続に使用される一意のテナント ID [!DNL Data Explorer] データベース。 |
| `servicePrincipalId` | に接続するために使用される一意のサービスプリンシパル ID [!DNL Data Explorer] データベース。 |
| `servicePrincipalKey` | への接続に使用する一意のサービスプリンシパルキー [!DNL Data Explorer] データベース。 |

の導入について詳しくは、 [この [!DNL Data Explorer] 文書](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad).

## [!DNL Azure Data Explorer] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Data Explorer] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 この **[!UICONTROL カタログ]** 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL AzureData Explorer]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** をクリックして、新しいData Explorerコネクタを作成します。

![カタログ](../../../../images/tutorials/create/data-explorer/catalog.png)

この **[!UICONTROL AzureData Explorerに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL Data Explorer] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/data-explorer/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Data Explorer] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/data-explorer/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Data Explorer] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
