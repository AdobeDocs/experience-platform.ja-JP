---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Big Query;google big query;GBQ;gbq
solution: Experience Platform
title: UI でのGoogleビッグクエリソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してGoogleビッグクエリソース接続を作成する方法を説明します。
exl-id: 3c0902de-48b9-42d8-a4bd-0213ca85fc7f
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 11%

---

# UI での [!DNL Google Big Query] ソース接続の作成

>[!NOTE]
>
> [!DNL Google BigQuery] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Google Big Query]（以下「BigQuery」と呼ばれます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な BigQuery 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL Platform] の BigQuery アカウントにアクセスするには、次の OAuth 2.0 認証値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `project` | クエリを実行するデフォルトの [!DNL BigQuery] プロジェクトのプロジェクト ID。 |
| `clientID` | 更新トークンの生成に使用する ID 値。 |
| `clientSecret` | 更新トークンの生成に使用するシークレット値。 |
| `refreshToken` | [!DNL Google] から取得した更新トークン。[!DNL BigQuery] へのアクセスを承認するために使用されます。 |

これらの値の詳細は、[ この BigQuery ドキュメント ](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing) を参照してください。

## Google BigQuery アカウントの接続

必要な資格情報を収集したら、以下の手順に従って、BigQuery アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL Databases]**」カテゴリで、「**[!UICONTROL Google Big Query]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい BigQuery コネクタを作成します。

![](../../../../images/tutorials/create/google-big-query/catalog.png)

「**[!UICONTROL Googleビッグクエリに接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、BigQuery 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/google-big-query/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する BigQuery アカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![](../../../../images/tutorials/create/google-big-query/existing.png)

## 次の手順

このチュートリアルに従って、GBQ アカウントへの接続を確立しました。 次のチュートリアルに進み、[ にデータを取り込むようにデータフローを設定します。 [!DNL Platform]](../../dataflow/databases.md)
