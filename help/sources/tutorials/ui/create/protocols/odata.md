---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIで汎用ODataソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 598b29f681ac930a4e1781f7f298608c8344d807
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 15%

---


# Create a [!DNL Generic OData] source connector in the UI

>[!NOTE]
> コネクタ [!DNL Generic OData] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Platform] ザーインターフェイスを使用して汎用オープンデータプロトコル（以下「OData」という）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に有効なOData接続がある場合は、このドキュメントの残りの部分をスキップし、プロトコルデータセットフローの [設定に関するチュートリアルに進むことができます](../../dataflow/protocols.md)

### 必要な資格情報の収集

で [!DNL OData] アカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `url` | サービスのルートURL [!DNL OData] です。 |

使い始める前に詳しくは、 [このODataドキュメントを参照してください](https://www.odata.org/getting-started/basic-tutorial/)。

## アカウントに接続 [!DNL OData] する

必要な資格情報を収集したら、次の手順に従って、接続する新しい [!DNL OData] アカウントを作成でき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 「 *[!UICONTROL カタログ]* 」画面には様々なソースが表示され、このソースから受信アカウントを作成できます。 各ソースには、既存のアカウントの数と、それらに関連付けられたデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL プロトコル]* 」カテゴリの下で、「 **[!UICONTROL 汎用OData]** 」を選択して、情報バーを画面の右側に表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信接続を作成するには、 **[!UICONTROL 追加「]** data」を選択します。

![カタログ](../../../../images/tutorials/create/odata/catalog.png)

「 *[!UICONTROL 汎用ODataに]* 接続」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明および [!DNL OData] 資格情報を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/odata/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL OData] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/odata/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL OData] カウントへの接続を確立しました。 次のチュートリアルに進み、プロトコルデータをプラットフォームに取り込むためのデータセットフローを [設定できるようになりました](../../dataflow/protocols.md)。