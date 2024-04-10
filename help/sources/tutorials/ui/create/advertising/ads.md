---
title: UI でのGoogle Ads ソースコネクタの作成
description: Adobe Experience Platform UI を使用してGoogle Ads ソース接続を作成する方法を説明します。
exl-id: 33dd2857-aed3-4e35-bc48-1c756a8b3638
source-git-commit: ce3dabe4ab08a41e581b97b74b3abad352e3267c
workflow-type: tm+mt
source-wordcount: '680'
ht-degree: 28%

---

# UI でのGoogle Ads ソースコネクタの作成

>[!WARNING]
>
>この [!DNL Google Ads] ソースは一時的に使用できません。 Adobeはこのソースに関する問題を解決するために取り組んでいます。

>[!NOTE]
>
>Google Ads ソースはベータ版です。 を参照してください。 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きソースの使用の詳細については、を参照してください。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用してGoogle Ads ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効なGoogle Ads 接続がある場合は、このドキュメントの残りの部分をスキップして、でチュートリアルに進むことができます。 [データフローの設定](../../dataflow/advertising.md)

### 必要な資格情報の収集

Google Ads アカウントプラットフォームにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| クライアント顧客 ID | クライアントカスタマー ID は、Google Ads API で管理するGoogle Ads クライアントアカウントに対応するアカウント番号です。 この ID は、のテンプレートに従います `123-456-7890`. |
| ログイン カスタマー ID | ログインカスタマー ID は、Google Ads Manager のアカウントに対応するアカウント番号で、特定の運用顧客からレポートデータを取得するために使用されます。 ログイン顧客 ID について詳しくは、を参照してください。 [Google Ads API ドキュメント](https://developers.google.com/search-ads/reporting/concepts/login-customer-id). |
| 開発者トークン | 開発者トークンを使用すると、Google Ads API にアクセスできます。 同じ開発者トークンを使用して、すべてのGoogle Ads アカウントに対してリクエストを行うことができます。 による開発者トークンの取得 [manager アカウントへのログイン](https://ads.google.com/home/tools/manager-accounts/) 次に、API センターページに移動します。 |
| 更新トークン | 更新トークンはの一部です [!DNL OAuth2] 認証。 このトークンを使用すると、有効期限が切れた後にアクセストークンを再生成できます。 |
| クライアント ID | クライアント ID は、クライアント秘密鍵と並行して、の一部として使用されます [!DNL OAuth2] 認証。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleに対するアプリケーションを識別し、お客様のアカウントに代わってアプリケーションを動作させることができます。 |
| クライアントシークレット | クライアント秘密鍵は、クライアント ID と並行して次の目的で使用されます [!DNL OAuth2] 認証。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleに対するアプリケーションを識別し、お客様のアカウントに代わってアプリケーションを動作させることができます。 |

の API の概要ドキュメントを参照してください [Google Ads の使用の手引きの詳細情報](https://developers.google.com/google-ads/api/docs/first-call/overview).

## Google Ads アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 **[!UICONTROL 広告]** カテゴリ、選択 **[!UICONTROL Google広告]**&#x200B;を選択してから、 **[!UICONTROL データを追加]**.

![Experience PlatformUI のソースカタログ。](../../../../images/tutorials/create/ads/catalog.png).

この **[!UICONTROL Google Ads への接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続するGoogle Ads アカウントを選択し、 **[!UICONTROL 次]** をクリックして続行します。

![ソースワークフローの既存のアカウントの選択ページ。](../../../../images/tutorials/create/ads/existing.png).

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、Google Ads の資格情報を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ソースワークフローの新しいアカウントインターフェイス。](../../../../images/tutorials/create/ads/new.png).

## 次の手順

このチュートリアルでは、Google Ads アカウントとの接続を確立しました。 次のチュートリアルに進むことができます。 [広告データを Platform に取り込むためのデータフローの設定](../../dataflow/advertising.md).
