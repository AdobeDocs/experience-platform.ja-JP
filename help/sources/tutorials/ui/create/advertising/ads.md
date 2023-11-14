---
title: UI でのGoogle Ads ソース接続の作成
description: Adobe Experience Platform UI を使用してGoogle Ads ソース接続を作成する方法を説明します。
exl-id: 33dd2857-aed3-4e35-bc48-1c756a8b3638
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 28%

---

# UI でのGoogle Ads ソース接続の作成

>[!NOTE]
>
>Google Ads ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

このチュートリアルでは、Adobe Experience Platformユーザーインターフェイスを使用してGoogle Ads ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効なGoogle Ads 接続がある場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/advertising.md)

### 必要な資格情報の収集

Google Ads アカウントプラットフォームにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| クライアント顧客 ID | クライアント顧客 ID は、Google Ads API で管理するGoogle Ads クライアントアカウントに対応するアカウント番号です。 この ID は、 `123-456-7890`. |
| ログイン顧客 ID | ログイン顧客 ID は、Google Ads Manager アカウントに対応するアカウント番号で、特定のオペレーティング顧客からレポートデータを取得するために使用されます。 ログイン顧客 ID について詳しくは、 [Google Ads API ドキュメント](https://developers.google.com/google-ads/api/docs/migration/login-customer-id). |
| 開発者トークン | 開発者トークンを使用すると、Google Ads API にアクセスできます。 同じ開発者トークンを使用して、すべてのGoogle Ads アカウントに対してリクエストを実行できます。 次の方法で開発者トークンを取得します： [マネージャーアカウントへのログイン](https://ads.google.com/home/tools/manager-accounts/) 次に、API センターページに移動します。 |
| 更新トークン | 更新トークンは、 [!DNL OAuth2] 認証。 このトークンを使用すると、期限切れになったアクセストークンを再生成できます。 |
| クライアント ID | クライアント ID は、 [!DNL OAuth2] 認証。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleへのアプリケーションを識別し、アカウントに代わってアプリケーションを操作できます。 |
| クライアントシークレット | クライアントの秘密鍵は、 [!DNL OAuth2] 認証。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleへのアプリケーションを識別し、アカウントに代わってアプリケーションを操作できます。 |

API の概要に関するドキュメント ( [Google Ads の使用開始に関する詳細](https://developers.google.com/google-ads/api/docs/first-call/overview).

## Google Ads アカウントの接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 **[!UICONTROL 広告]** カテゴリ、選択 **[!UICONTROL Google Ads]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![ソースカタログ (Experience PlatformUI)。](../../../../images/tutorials/create/ads/catalog.png).

The **[!UICONTROL Google Ads に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続するGoogle Ads アカウントを選択し、「 **[!UICONTROL 次へ]** をクリックして続行します。

![ソースワークフロー内の既存のアカウントの選択ページ。](../../../../images/tutorials/create/ads/existing.png).

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）およびGoogle Ads の資格情報を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ソースワークフローの新しいアカウントインターフェイス。](../../../../images/tutorials/create/ads/new.png).

## 次の手順

このチュートリアルに従って、Google Ads アカウントへの接続を確立しました。 次のチュートリアルに進み、 [広告データを Platform に取り込むためのデータフローの設定](../../dataflow/advertising.md).
