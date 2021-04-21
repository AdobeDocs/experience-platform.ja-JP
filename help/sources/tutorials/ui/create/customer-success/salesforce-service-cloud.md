---
keywords: Experience Platform；ホーム；人気のあるトピック；Salesforce Service Cloud;salesforceサービスクラウド
solution: Experience Platform
title: UIでのSalesforceサービスクラウドソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してSalesforceサービスクラウドのソース接続を作成する方法を説明します。
exl-id: 38480a29-7852-46c6-bcea-5dc6bffdbd15
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 9%

---

# UIに[!DNL Salesforce Service Cloud]ソース接続を作成する

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Salesforce Service Cloud] （以下「SSC」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なSSC接続がある場合は、このドキュメントの残りの部分をスキップし、[データフロー](../../dataflow/customer-success.md)の設定のチュートリアルに進むことができます

### 必要な資格情報の収集

[!DNL Platform]のSSCアカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `username` | ユーザーアカウントのユーザー名です。 |
| `password` | ユーザーアカウントのパスワードです。 |
| `securityToken` | ユーザーアカウントのセキュリティトークン。 |

開始方法の詳細については、[この [!DNL Salesforce Service Cloud] ドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_iot.meta/api_iot/qs_auth_access_token.htm)を参照してください。

## [!DNL Salesforce Service Cloud]アカウントに接続

必要な資格情報を収集したら、次の手順に従ってSSCアカウントを[!DNL Platform]にリンクできます。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「**[!UICONTROL Customer Success]**」カテゴリで、「**[!UICONTROL Salesforce Service Cloud]**」を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しいSSCコネクタを作成します。

![カタログ](../../../../images/tutorials/create/ssc/catalog.png)

**[!UICONTROL Salesforce Service Cloud]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームに、名前、オプションの説明、およびSSC証明書を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ssc/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するSSCアカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ssc/existing.png)

## 次の手順

このチュートリアルに従うことで、SSCアカウントへの接続を確立できました。 次のチュートリアルに進み、[顧客の成功データを [!DNL Platform]](../../dataflow/customer-success.md)に送るようにデータフローを設定できます。
