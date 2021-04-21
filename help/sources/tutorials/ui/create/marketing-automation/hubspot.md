---
keywords: Experience Platform；ホーム；人気のあるトピック；ハブスポット；ハブスポット
solution: Experience Platform
title: UIでのHubSpotソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してHubSpotソース接続を作成する方法を説明します。
exl-id: 452b7290-b9e8-4728-8b58-0e0c76bd9449
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 9%

---

# UIに[!DNL HubSpot]ソース接続を作成する

>[!NOTE]
>
> [!DNL HubSpot]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL HubSpot]ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に[!DNL HubSpot]ドキュメントをお持ちの場合は、この接続の残りの部分をスキップして、[データフロー](../../dataflow/marketing-automation.md)の設定に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform]の[!DNL HubSpot]アカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `clientId` | [!DNL HubSpot]アプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | [!DNL HubSpot]アプリケーションに関連付けられているクライアントシークレット。 |
| `accessToken` | OAuth統合を最初に認証する際に取得されるアクセストークン。 |
| `refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |

開始方法の詳細については、[[!DNL HubSpot] ドキュメント](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview)を参照してください。

## [!DNL HubSpot]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL HubSpot]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL マーケティング自動化]**&#x200B;カテゴリの下で、**[!UICONTROL HubSpot]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL HubSpot]コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hubspot/catalog.png)

**[!UICONTROL Connect to HubSpot]**&#x200B;ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL HubSpot]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/hubspot/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL HubSpot]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/hubspot/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL HubSpot]アカウントへの接続が確立されます。 次のチュートリアルに進み、[マーケティング自動化システムデータを [!DNL Platform]](../../dataflow/marketing-automation.md)に取り込むようにデータフローを設定できます。
