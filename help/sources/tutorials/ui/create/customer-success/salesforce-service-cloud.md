---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UI での Salesforce サービスクラウドソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 16%

---


# Create a [!DNL Salesforce Service Cloud] source connector in the UI

>[!NOTE]
>コネクタ [!DNL Salesforce Service Cloud] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Salesforce Service Cloud][!DNL Platform] ザーインターフェイスを使用して、SSC （以下、「SSC」）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に有効なSSC接続がある場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/customer-success.md)

### 必要な資格情報の収集

でSSCアカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `username` | ユーザーアカウントのユーザー名です。 |
| `password` | ユーザーアカウントのパスワードです。 |
| `securityToken` | ユーザーアカウントのセキュリティトークン。 |

使い始める前に詳しくは、 [このSalesforce Service Cloudドキュメントを参照してください](https://developer.salesforce.com/docs/atlas.en-us.api_iot.meta/api_iot/qs_auth_access_token.htm)。

## アカウントに接続 [!DNL Salesforce Service Cloud] する

必要な資格情報を収集したら、次の手順に従って、接続先の新しいSSCアカウントを作成でき [!DNL Platform]ます。

「 <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platform</a> 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 カ *[!UICONTROL タログ]* 画面には様々なソースが表示され、このソースから受信アカウントを作成できます。各ソースには既存のアカウントの数と関連するデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL Customer Success]* ( **[!UICONTROL 顧客成功]** )」カテゴリの下で、「Salesforce Service Cloud」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信接続を作成するには、「 **[!UICONTROL 接続ソース]**」を選択します。

![カタログ](../../../../images/tutorials/create/ssc/catalog.png)

「Salesforce Service Cloud *[!UICONTROL に接続]* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明、SSC証明書を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ssc/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するSSCアカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ssc/existing.png)

## 次の手順

このチュートリアルに従うことで、SSCアカウントへの接続を確立できました。 次のチュートリアルに進み、顧客の成功データをPlatformに導くデータフローを [設定できるようになりました](../../dataflow/customer-success.md)。