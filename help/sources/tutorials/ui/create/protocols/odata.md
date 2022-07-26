---
keywords: Experience Platform；ホーム；人気の高いトピック；OData;odata;Generic Open Data Protocol
solution: Experience Platform
title: UI での汎用 OData ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して汎用オープンデータプロトコルのソース接続を作成する方法を説明します。
exl-id: aad6b6f7-622c-4ab6-bf6d-1221fe1132d1
source-git-commit: 1e2644b7d83a0bcb7175f27d7c4859c0efba4060
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 46%

---

# UI での [!DNL Generic OData] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Generic Open Data Protocol] （以下「」という。）[!DNL OData]&quot;) を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL OData] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/protocols.md)

### 必要な認証情報の収集

次の項目にアクセスするには、 [!DNL OData] アカウント [!DNL Platform]に値を指定する場合は、次の値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `url` | のルート URL [!DNL OData] サービス。 |

の導入について詳しくは、 [この [!DNL OData] 文書](https://www.odata.org/getting-started/basic-tutorial/).

## [!DNL OData] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL OData] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL プロトコル]** カテゴリ、選択 **[!UICONTROL 汎用 OData]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL OData] コネクタ。

![カタログ](../../../../images/tutorials/create/odata/catalog.png)

この **[!UICONTROL 汎用 OData に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、接続に名前、オプションの説明を入力し、 [!DNL OData] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/odata/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL OData] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/odata/existing.png)

## 次の手順

このチュートリアルでは、[!DNL OData] アカウントとの接続を確立しました。次のチュートリアルに進み、 [プロトコルデータをに取り込むようにデータフローを設定する [!DNL Platform]](../../dataflow/protocols.md).
