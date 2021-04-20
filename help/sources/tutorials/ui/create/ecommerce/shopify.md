---
keywords: Experience Platform；ホーム；人気の高いトピック；shopify;Shopify
solution: Experience Platform
title: UIでのShopifyソース接続の作成
topic: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してShopifyソース接続を作成する方法を説明します。
translation-type: tm+mt
source-git-commit: cc23228cb410dc4c70a56c5142be00c2ca1c40d3
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 13%

---


# UIに[!DNL Shopify]ソース接続を作成する

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Shopify]ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に[!DNL Shopify]接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、[eコマースコネクタ](../../dataflow/ecommerce.md)のデータフローの設定に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform]の[!DNL Shopify]アカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Shopify]サーバーのエンドポイント。 |
| `accessToken` | [!DNL Shopify]ユーザーアカウントのアクセストークンです。 |

開始方法の詳細については、[[!DNL Shopify] ドキュメント](https://shopify.dev/concepts/about-apis/authentication)を参照してください。

## [!DNL Shopify]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL Shopify]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL eCommerce]**&#x200B;カテゴリの下で、「**[!UICONTROL Shopify]**」を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL Shopify]コネクタを作成します。

![カタログ](../../../../images/tutorials/create/shopify/catalog.png)

**[!UICONTROL Shopify]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL Shopify]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/shopify/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL Shopify]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/shopify/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL Shopify]アカウントへの接続が確立されます。 次のチュートリアルに進み、eCommerceデータを [!DNL Platform]](../../dataflow/ecommerce.md)に取り込むように[データフローを設定できます。