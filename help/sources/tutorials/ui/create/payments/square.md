---
keywords: Experience Platform；ホーム；人気のトピック；正方形；正方形
title: UI での Square Source接続の作成
description: Adobe Experience Platform UI を使用して Square ソースコネクタを作成する方法を説明します。
exl-id: 7cdfeb36-c989-4875-bb94-e6594ddf30da
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 44%

---

# UI での [!DNL Square] ソース接続の作成

このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用して [!DNL Square] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

### 必要な資格情報の収集

[!DNL Square] アカウントのExperience Platformにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| ホスト | [!DNL Square] インスタンスの URL。 |
| クライアント ID | [!DNL Square] アカウントに関連付けられたクライアント ID。 |
| クライアントシークレット | [!DNL Square] アカウントに関連付けられたクライアントの秘密鍵。 |
| アクセストークン | アクセストークンは、OAuth 2.0 認証で [!DNL Square] アカウントを認証するために使用されます。 アクセストークンは [!DNL Square] から取得できます。 |
| 更新トークン | 更新トークンは、現在のアクセストークンの有効期限が切れた後に新しいアクセストークンを生成するために使用されます。 更新トークンは [!DNL Square] から取得できます。 |

これらの資格情報とその取得方法について詳しくは、[[!DNL Square] OAuth に関するドキュメント &#x200B;](https://developer.squareup.com/docs/oauth-api/receive-and-manage-tokens) を参照してください。

必要な資格情報を収集したら、次の手順に従って [!DNL Square] アカウントをExperience Platformにリンクできます。

## [!DNL Square] アカウントを接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL &#x200B; 支払い &#x200B;] カテゴリで、「**[!UICONTROL Square]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/square/catalog.png)

**[!UICONTROL Square に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Square] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/square/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、[!DNL Square] しい資格情報の名前、説明（オプション）、適切な値を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/square/new.png)

## 次の手順

このチュートリアルでは、認証を行い、お使いの [!DNL Square] アカウントとExperience Platformとのソース接続を作成しました。 次のチュートリアルに進み、[&#x200B; 支払いデータをExperience Platformに取り込むためのデータフローの作成 &#x200B;](../../dataflow/payments.md) を行いましょう。
