---
keywords: Experience Platform；ホーム；人気のあるトピック；hubspot;Hubspot
solution: Experience Platform
title: UI での HubSpot ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して HubSpot ソース接続を作成する方法を説明します。
exl-id: 452b7290-b9e8-4728-8b58-0e0c76bd9449
source-git-commit: e150f05df2107d7b3a2e95a55dc4ad072294279e
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 12%

---

# UI での [!DNL HubSpot] ソース接続の作成

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL HubSpot] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL HubSpot] 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/marketing-automation.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL Platform] の [!DNL HubSpot] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `clientId` | [!DNL HubSpot] アプリケーションに関連付けられているクライアント ID。 |
| `clientSecret` | [!DNL HubSpot] アプリケーションに関連付けられているクライアントの秘密鍵。 |
| `accessToken` | OAuth 統合を最初に認証したときに取得されるアクセストークン。 |
| `refreshToken` | OAuth 統合を最初に認証したときに取得される更新トークン。 |

使い始める方法については、この [[!DNL HubSpot]  ドキュメント ](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview) を参照してください。

## [!DNL HubSpot] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って [!DNL HubSpot] アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL マーケティングの自動化]**」カテゴリで、「**[!UICONTROL HubSpot]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して新しい [!DNL HubSpot] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hubspot/catalog.png)

**[!UICONTROL HubSpot に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、[!DNL HubSpot] 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/hubspot/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL HubSpot] アカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![既存](../../../../images/tutorials/create/hubspot/existing.png)

## 次の手順

このチュートリアルに従って、[!DNL HubSpot] アカウントへの接続を確立しました。 次のチュートリアルに進み、マーケティング自動化システムデータを  [!DNL Platform]](../../dataflow/marketing-automation.md) に取り込むように [ データフローを設定します。
