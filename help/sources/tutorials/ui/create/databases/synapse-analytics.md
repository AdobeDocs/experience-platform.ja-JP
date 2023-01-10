---
keywords: Experience Platform；ホーム；人気の高いトピック；Azure synapse分析；Synapse;Synapse;azure synapse分析
solution: Experience Platform
title: UI でのAzure synapseAnalytics ソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、Azure synapse分析（以下「Synapse」と呼ばれます）ソース接続を作成する方法について説明します。
exl-id: 1f1ce317-eaaf-4ad2-a5fb-236983220bd7
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 58%

---

# UI での [!DNL Azure Synapse Analytics] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Azure Synapse Analytics] （以下「」という。）[!DNL Synapse]&quot;) を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Synapse] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

 で [!DNL Synapse] アカウントにアクセスするには、次の値を指定する必要があります。[!DNL Platform]

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | 次に示すように、 [!DNL Synapse] 認証。 この [!DNL Synapse] 接続文字列のパターンは次のとおりです。 `Server=tcp:{SERVER_NAME}.database.windows.net,1433;Database={DATABASE};User ID={USERNAME}@{SERVER_NAME};Password={PASSWORD};Trusted_Connection=False;Encrypt=True;Connection Timeout=30`. |

この値について詳しくは、 [この [!DNL Synapse] 文書](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-sql-data-warehouse).

## [!DNL Synapse] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Synapse] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL azure synapse分析]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL Synapse] コネクタ。

![](../../../../images/tutorials/create/azure-synapse-analytics/catalog.png)

この **[!UICONTROL azure synapse分析に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）および [!DNL Synapse] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/azure-synapse-analytics/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Synapse] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![](../../../../images/tutorials/create/azure-synapse-analytics/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Synapse] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
