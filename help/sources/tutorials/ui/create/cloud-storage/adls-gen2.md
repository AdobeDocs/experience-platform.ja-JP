---
keywords: Experience Platform；ホーム；人気のトピック；Azure Data Lake Storage Gen2;ADLS Gen2;adls gen2;adls コネクタ
solution: Experience Platform
title: UI での Azure Data Lake Storage Gen2 Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Azure Data Lake Storage Gen2 ソース接続を作成する方法を説明します。
exl-id: d81b7593-08a3-43f8-a8bc-f5547a6cd55a
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 33%

---

# UI での [!DNL Azure Data Lake Storage Gen2] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] ユーザーインターフェイスを使用して [!DNL Azure Data Lake Storage Gen2] （以下「[!DNL ADLS Gen2]」）ソースコネクタを認証する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な ADLS Gen2 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/batch/cloud-storage.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL ADLS Gen2] ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | [!DNL ADLS Gen2] のエンドポイント。 |
| `servicePrincipalId` | アプリケーションのクライアント ID。 |
| `servicePrincipalKey` | アプリケーションのキー。 |
| `tenant` | アプリケーションを含むテナント情報。 |

これらの値について詳しくは、[ この  [!DNL ADLS Gen2]  ドキュメント ](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage) を参照してください。

## [!DNL ADLS Gen2] アカウントを接続

必要な資格情報を収集したら、次の手順に従って [!DNL ADLS Gen2] アカウントを [!DNL Experience Platform] に接続するようにリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL データベース]** カテゴリで、「**[!UICONTROL Azure Data Lake Gen2]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい ADLS Gen2 コネクタを作成します。

![](../../../../images/tutorials/create/adls-gen2/catalog.png)

**[!UICONTROL Azure Data Lake Gen2 に接続]** ダイアログが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、[!DNL ADLS Gen2] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![](../../../../images/tutorials/create/adls-gen2/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL ADLS Gen2] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![](../../../../images/tutorials/create/adls-gen2/existing.png)

## 次の手順

このチュートリアルでは、[!DNL ADLS Gen2] アカウントとの接続を確立しました。次のチュートリアルに進み、[ クラウドストレージからデータをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/batch/cloud-storage.md) を行いましょう。
