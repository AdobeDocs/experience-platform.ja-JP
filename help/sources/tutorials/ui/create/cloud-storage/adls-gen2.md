---
keywords: Experience Platform；ホーム；人気の高いトピック；Azure Data Lake Storage Gen2;ADLS Gen2;adls gen2;adls コネクタ
solution: Experience Platform
title: UI での Azure データレイクストレージ Gen2 ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Azure データレイクストレージ Gen2 ソース接続を作成する方法を説明します。
exl-id: d81b7593-08a3-43f8-a8bc-f5547a6cd55a
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 37%

---

# の作成 [!DNL Azure Data Lake Storage Gen2] UI のソース接続

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Azure Data Lake Storage Gen2] （以下「」という。）[!DNL ADLS Gen2]&quot;) を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な ADLS Gen2 接続がある場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/batch/cloud-storage.md).

### 必要な資格情報の収集

を認証するために [!DNL ADLS Gen2] ソースコネクタの場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | のエンドポイント [!DNL ADLS Gen2]. |
| `servicePrincipalId` | アプリケーションのクライアント ID。 |
| `servicePrincipalKey` | アプリケーションのキー。 |
| `tenant` | アプリケーションを含むテナント情報。 |

これらの値について詳しくは、 [この [!DNL ADLS Gen2] 文書](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage).

## [!DNL ADLS Gen2] アカウントを接続

必要な資格情報を収集したら、次の手順に従って、 [!DNL ADLS Gen2] 接続するアカウント [!DNL Platform].

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Azure Data Lake Gen2]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** をクリックして、新しい ADLS Gen2 コネクタを作成します。

![](../../../../images/tutorials/create/adls-gen2/catalog.png)

この **[!UICONTROL Azure Data Lake Gen2 に接続]** ダイアログが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新規アカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL ADLS Gen2] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/adls-gen2/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL ADLS Gen2] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![](../../../../images/tutorials/create/adls-gen2/existing.png)

## 次の手順

このチュートリアルでは、[!DNL ADLS Gen2] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データフローを設定して、クラウドストレージからにデータを取り込みます。 [!DNL Platform]](../../dataflow/batch/cloud-storage.md).
