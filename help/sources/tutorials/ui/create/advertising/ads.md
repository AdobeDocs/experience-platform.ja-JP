---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのGoogle AdWordsソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 14%

---


# Create a [!DNL Google AdWords] source connector in the UI

>[!NOTE]
>コネクタ [!DNL Google AdWords] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Google AdWords] ザインタフェースを使用して [!DNL Platform] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に接続している場合は、このドキュメントの残りの部分をスキップして、データフローの [!DNL Google AdWords][設定に関するチュートリアルに進むことができます](../../dataflow/payments.md)

### 必要な資格情報の収集

ア [!DNL Google AdWords] カウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `clientCustomerId` | AdWordsアカウントのクライアント顧客ID。 |
| `developerToken` | マネージャーアカウントに関連付けられている開発者トークン。 |
| `refreshToken` | AdWordsへのアクセスを許可す [!DNL Google] るために取得した更新トークン。 |
| `clientId` | 更新トークンの取得に使用される [!DNL Google] アプリケーションのクライアントID。 |
| `clientSecret` | 更新トークンの取得に使用される [!DNL Google] アプリケーションのクライアントシークレット。 |

使い始める前に、この [Google AdWordsドキュメントを参照してください](https://developers.google.com/adwords/api/docs/guides/authentication)。

## アカウントに接続 [!DNL Google AdWords] する

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、ア [!DNL Google AdWords] カウントをリンクし [!DNL Platform]ます。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 [ *[!UICONTROL カタログ]* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *[!UICONTROL 広告]* 」カテゴリの下で「 **[!UICONTROL Google AdWords]** 」を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **[!UICONTROL 接続ソース]**」を選択します。

![カタログ](../../../../images/tutorials/create/ads/catalog.png)

「Google AdWords *[!UICONTROL に接続]* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明および [!DNL Google AdWords] 資格情報を指定します。 終了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ads/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Google AdWords] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ads/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Google AdWords] カウントへの基本的な接続を確立しました。 次のチュートリアルに進み、広告データをPlatformに取り込むようにデータフローを [設定できるようになりました](../../dataflow/advertising.md)。