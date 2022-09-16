---
keywords: Experience Platform；ホーム；人気のトピック；hubspot;Hubspot
solution: Experience Platform
title: UI での HubSpot ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して HubSpot ソース接続を作成する方法を説明します。
exl-id: 452b7290-b9e8-4728-8b58-0e0c76bd9449
source-git-commit: e150f05df2107d7b3a2e95a55dc4ad072294279e
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 50%

---

# UI での [!DNL HubSpot] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] のユーザーインターフェイスを使用して [!DNL HubSpot] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL HubSpot] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/marketing-automation.md).

### 必要な認証情報の収集

 で [!DNL HubSpot] アカウントにアクセスするには、次の値を指定する必要があります。[!DNL Platform]

| 認証情報 | 説明 |
| ---------- | ----------- |
| `clientId` | 次に関連付けられたクライアント ID: [!DNL HubSpot] アプリケーション。 |
| `clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL HubSpot] アプリケーション。 |
| `accessToken` | OAuth 統合を最初に認証したときに取得されたアクセストークン。 |
| `refreshToken` | OAuth 統合の初回認証時に取得された更新トークン。 |

導入の詳細については、 [[!DNL HubSpot] 文書](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview).

## [!DNL HubSpot] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL HubSpot] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL マーケティングの自動化]** カテゴリ、選択 **[!UICONTROL HubSpot]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL HubSpot] コネクタ。

![カタログ](../../../../images/tutorials/create/hubspot/catalog.png)

この **[!UICONTROL HubSpot に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL HubSpot] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/hubspot/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL HubSpot] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/hubspot/existing.png)

## 次の手順

このチュートリアルでは、[!DNL HubSpot] アカウントとの接続を確立しました。次のチュートリアルに進み、 [マーケティング自動化システムデータをに取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/marketing-automation.md).
