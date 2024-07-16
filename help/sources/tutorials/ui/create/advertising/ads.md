---
title: UI でのGoogle Ads Source接続の作成
description: Adobe Experience Platform UI を使用してGoogle Ads ソース接続を作成する方法を説明します。
exl-id: 33dd2857-aed3-4e35-bc48-1c756a8b3638
source-git-commit: ce3dabe4ab08a41e581b97b74b3abad352e3267c
workflow-type: tm+mt
source-wordcount: '680'
ht-degree: 29%

---

# UI でのGoogle Ads ソースコネクタの作成

>[!WARNING]
>
>[!DNL Google Ads] ソースは一時的に使用できません。 Adobeはこのソースに関する問題を解決するために取り組んでいます。

>[!NOTE]
>
>Google Ads ソースはベータ版です。 ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用してGoogle Ads ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効なGoogle Ads 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/advertising.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

Google Ads アカウントプラットフォームにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| クライアント顧客 ID | クライアントカスタマー ID は、Google Ads API で管理するGoogle Ads クライアントアカウントに対応するアカウント番号です。 この ID は `123-456-7890` のテンプレートに従います。 |
| ログイン カスタマー ID | ログインカスタマー ID は、Google Ads Manager のアカウントに対応するアカウント番号で、特定の運用顧客からレポートデータを取得するために使用されます。 ログインカスタマー ID について詳しくは、[Google Ads API ドキュメント ](https://developers.google.com/search-ads/reporting/concepts/login-customer-id) を参照してください。 |
| 開発者トークン | 開発者トークンを使用すると、Google Ads API にアクセスできます。 同じ開発者トークンを使用して、すべてのGoogle Ads アカウントに対してリクエストを行うことができます。 [ マネージャーアカウントにログイン ](https://ads.google.com/home/tools/manager-accounts/) して API センターページに移動して、開発者トークンを取得します。 |
| 更新トークン | 更新トークンは認証の一部 [!DNL OAuth2] す。 このトークンを使用すると、有効期限が切れた後にアクセストークンを再生成できます。 |
| クライアント ID | クライアント ID は、クライアント秘密鍵と並行して、認証の一部として使用 [!DNL OAuth2] れます。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleに対するアプリケーションを識別し、お客様のアカウントに代わってアプリケーションを動作させることができます。 |
| クライアントシークレット | クライアントの秘密鍵は、クライアント ID と並行して、認証の一部として使用 [!DNL OAuth2] れます。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleに対するアプリケーションを識別し、お客様のアカウントに代わってアプリケーションを動作させることができます。 |

詳しくは、API の概要のドキュメント [Google Ads の使用の手引きについて詳しくは ](https://developers.google.com/google-ads/api/docs/first-call/overview) を参照してください。

## Google Ads アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Advertising]** カテゴリで、「**[!UICONTROL Google広告]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![Experience PlatformUI のソースカタログ ](../../../../images/tutorials/create/ads/catalog.png)

**[!UICONTROL Google Ads への接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続するGoogle Ads アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![ ソースワークフローの既存のアカウントの選択ページ ](../../../../images/tutorials/create/ads/existing.png)..

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、Google Ads の資格情報を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ ソースワークフローの新しいアカウントインターフェイス ](../../../../images/tutorials/create/ads/new.png)

## 次の手順

このチュートリアルでは、Google Ads アカウントとの接続を確立しました。 次のチュートリアルに進み、[ 広告データを Platform に取り込むためのデータフローの設定 ](../../dataflow/advertising.md) を行いましょう。
