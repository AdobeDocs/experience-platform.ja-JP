---
keywords: Experience Platform；ホーム；人気のトピック；shopify;Shopify
solution: Experience Platform
title: UI での Shopify Source連携の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Shopify ソース接続を作成する方法を説明します。
exl-id: 527cac95-3d9a-4089-98e4-66d746641b85
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 45%

---

# UI での [!DNL Shopify] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] のユーザーインターフェイスを使用して [!DNL Shopify] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; エクスペリエンスデータモデル（XDM）システム &#x200B;](../../../../../xdm/home.md):[!DNL Experience Platform] が顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL Shopify] 接続がある場合は、このドキュメントの残りの部分をスキップし、[e コマースコネクタのデータフローの設定 &#x200B;](../../dataflow/ecommerce.md) に関するチュートリアルに進んで構いません。

### 必要な資格情報の収集

[!DNL Experience Platform] で [!DNL Shopify] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Shopify] サーバーのエンドポイント。 |
| `accessToken` | [!DNL Shopify] ユーザーアカウントのアクセストークン。 |

基本について詳しくは、この [[!DNL Shopify]  ドキュメント &#x200B;](https://shopify.dev/concepts/about-apis/authentication) を参照してください。

## [!DNL Shopify] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL Shopify] アカウントを [!DNL Experience Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL e コマース]** カテゴリの下で、「**[!UICONTROL Shopify]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Shopify] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/shopify/catalog.png)

**[!UICONTROL Shopify に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Shopify] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![&#x200B; 接続 &#x200B;](../../../../images/tutorials/create/shopify/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Shopify] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/shopify/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Shopify] アカウントとの接続を確立しました。次のチュートリアルに進み、[e コマースデータをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/ecommerce.md) を行いましょう。
