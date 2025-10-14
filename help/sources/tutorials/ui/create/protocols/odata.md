---
keywords: Experience Platform；ホーム；人気のトピック；OData;odata;Generic Open Data Protocol
solution: Experience Platform
title: UI での汎用 OData Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、汎用のオープンデータプロトコルソース接続を作成する方法を説明します。
exl-id: aad6b6f7-622c-4ab6-bf6d-1221fe1132d1
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 44%

---

# UI での [!DNL Generic OData] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] ユーザーインターフェイスを使用して [!DNL Generic Open Data Protocol] （以下「[!DNL OData]」）ソースコネクタを作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL OData] 接続がある場合は、このドキュメントの残りの部分をスキップして、[&#x200B; データフローの設定 &#x200B;](../../dataflow/protocols.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Experience Platform] で [!DNL OData] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | [!DNL OData] サービスのルート URL。 |

基本について詳しくは、[&#x200B; このドキュメント  [!DNL OData]  を参照してください &#x200B;](https://www.odata.org/getting-started/basic-tutorial/)。

## [!DNL OData] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL OData] アカウントを [!DNL Experience Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL プロトコル]** カテゴリで、「**[!UICONTROL 汎用 OData]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL OData] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/odata/catalog.png)

**[!UICONTROL 汎用 OData に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、接続、名前、説明（オプション）、[!DNL OData] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![&#x200B; 接続 &#x200B;](../../../../images/tutorials/create/odata/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL OData] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/odata/existing.png)

## 次の手順

このチュートリアルでは、[!DNL OData] アカウントとの接続を確立しました。次のチュートリアルに進み、[&#x200B; プロトコルデータをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/protocols.md) を行いましょう。
