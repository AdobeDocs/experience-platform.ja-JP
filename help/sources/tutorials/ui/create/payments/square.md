---
keywords: Experience Platform；ホーム；人気の高いトピック；正方形；四角形
title: UI での正方形ソース接続の作成
description: Adobe Experience Platform UI を使用して Square ソース接続を作成する方法を説明します。
source-git-commit: e905b11040c8de33aa757bd5743605bb36a8009b
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 51%

---

# UI での [!DNL Square] ソース接続の作成

このチュートリアルでは、 [!DNL Square] Platform ユーザーインターフェイスを使用したソースコネクタ

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

### 必要な認証情報の収集

次の項目にアクセスするには、 [!DNL Square] アカウントプラットフォームの場合は、次の値を指定する必要があります。

| 認証情報 | 説明 |
| --- | --- |
| ホスト | の URL [!DNL Square] インスタンス。 |
| クライアント ID | 次に関連付けられたクライアント ID: [!DNL Square] アカウント |
| クライアント秘密鍵 | に関連付けられたクライアント秘密鍵 [!DNL Square] アカウント |
| アクセストークン | アクセストークンは、 [!DNL Square] アカウントを OAuth 2.0 認証で使用します。 アクセストークンは、 [!DNL Square]. |
| 更新トークン | 更新トークンは、現在のアクセストークンの有効期限が切れた後に新しいアクセストークンを生成するために使用されます。 更新トークンは、 [!DNL Square]. |

これらの資格情報とその取得方法について詳しくは、 [[!DNL Square] OAuth に関するドキュメント](https://developer.squareup.com/docs/oauth-api/receive-and-manage-tokens).

必要な資格情報を収集したら、以下の手順に従って [!DNL Square] アカウントを Platform にリンクできます。

## [!DNL Square] アカウントを接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 [!UICONTROL 支払い] カテゴリ、選択 **[!UICONTROL 四角形]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/square/catalog.png)

この **[!UICONTROL Square に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Square] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/square/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、名前、説明（オプション）および [!DNL Square] 資格情報。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/square/new.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Square] アカウントとプラットフォーム。 次のチュートリアルに進み、 [データフローを作成して支払いデータを Platform に取り込む](../../dataflow/payments.md).
