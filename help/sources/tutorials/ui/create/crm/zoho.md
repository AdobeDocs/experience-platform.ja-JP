---
keywords: エクスペリエンス Platform、home、人気のある話題。Zoho CRM; Zoho crm;Zoho;zoho
solution: Experience Platform
title: UI での Zoho CRM ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe エクスペリエンスプラットフォーム UI を使用して Zoho CRM ソース接続を作成する方法について説明します。
source-git-commit: 7a15090d8ed2c1016d7dc4d7d3d0656640c4785c
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 11%

---

# [!DNL Zoho CRM]UI でのソース接続の作成

Adobe エクスペリエンスプラットフォームのソースコネクターには、指定された時間に外部の CRM データを取り込む機能が用意されています。 このチュートリアルでは [!DNL Zoho CRM] 、ユーザーインターフェイスを使用してソースコネクタを作成する方法について説明し [!DNL Platform] ます。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] System ](../../../../../xdm/home.md) : [!DNL Experience Platform] カスタマーエクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターチュートリアル ](../../../../../xdm/tutorials/create-schema-ui.md) : スキーマエディター UI を使用してカスタムスキーマを作成する方法について説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効なアカウントを持っている場合は、 [!DNL Zoho CRM] このドキュメントの残りの部分をスキップして、フローの設定に関するチュートリアルに進んで [ ](../../dataflow/crm.md) ください。

### 必要な資格情報の収集

プラットフォームに接続するには [!DNL Zoho CRM] 、次の接続プロパティの値を指定する必要があります。

| Chap | 説明 |
| --- | --- |
| Endpoint | [!DNL Zoho CRM]要求を出しているサーバーのエンドポイントを指定します。 |
| アカウント URL | Accounts URL は、アクセスおよび更新トークンを生成するために使用されます。 URL は、ドメイン固有である必要があります。 |
| クライアント ID | ユーザーアカウントに対応しているクライアント ID [!DNL Zoho CRM] 。 |
| クライアント秘密鍵 | このクライアントシークレットには、 [!DNL Zoho CRM] ユーザーアカウントが含まれています。 |
| アクセストークン | アクセストークンによって、アカウントに対するセキュリティで保護された安全なアクセスが認証され [!DNL Zoho CRM] ます。 |
| トークンの更新 | 更新トークンは、アクセストークンの有効期限が切れると、新しいアクセストークンを生成するために使用されるトークンです。 |

これらの資格情報について詳しくは、認証に関するドキュメントを参照してください [[!DNL Zoho CRM]  ](https://www.zoho.com/crm/developer/docs/api/v2/oauth-overview.html) 。

## アカウントの接続 [!DNL Zoho CRM]

必要な情報を収集したら、次の手順に従って [!DNL Zoho CRM] アカウントをにリンク [!DNL Platform] します。

プラットフォーム UI で、左側のナビゲーションバーの「ソース」を選択して、 **** [!UICONTROL  ソースワークスペースにアクセスし ] ます。 カタログ画面には、  アカウントを作成するための様々なソースが表示されます。

画面の左側のカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的のソースを検索することもできます。

「 [!UICONTROL  Crm ] 」カテゴリで、「Zoho crm」を選択し、 **** 「データを追加」を選択し **** ます。

![差し込み](../../../../images/tutorials/create/zoho/catalog.png)

「 **[!UICONTROL ZOHO CRM アカウントに接続」]** ページが表示されます。 このページでは、新しい資格情報を使用するか、既存の資格情報を使用することができます。

### 既存のアカウント

既存のアカウントを使用するには、新しいフローを作成するアカウントを選択し、「 [!DNL Zoho CRM] 次へ」を選択して先 **** に進みます。

![従来](../../../../images/tutorials/create/zoho/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新規アカウント」を選択]** し、名前、オプションの説明、および資格情報を入力し [!DNL Zoho CRM] ます。 終了したら、「ソースに接続」を選択し、 **** 新しい接続が確立されるまでしばらく待ちます。

>[!TIP]
>
>アカウント URL ドメインは、適切なドメインの場所に対応している必要があります。 次の例は、各ドメインとそれに対応するアカウント Url を示しています。<ul><li>日本: https://accounts.zoho.com</li><li>オーストラリア: https://accounts.zoho.com.au</li><li>ヨーロッパ: https://accounts.zoho.eu</li><li>インド: https://accounts.zoho.in</li><li>中国: https://accounts.zoho.com.cn</li></ul>

![新規](../../../../images/tutorials/create/zoho/new.png)

## 次の手順

このチュートリアルでは、お使いのアカウントへの接続が確立されてい [!DNL Zoho CRM] ます。 次のチュートリアルに進み、 [ データをプラットフォームに取り込むようにデータフローを設定することができ ](../../dataflow/crm.md) ます。
