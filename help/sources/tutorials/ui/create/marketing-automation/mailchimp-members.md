---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: Experience Platform UI を使用して MailChimp メンバーのソース接続を作成する
description: Experience Platform UI を使用してAdobe Experience Platformを MailChimp メンバーに接続する方法を説明します。
exl-id: dc620ef9-624d-4fc9-8475-bb475ea86eb7
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 63%

---

# Experience Platform UI を使用した [!DNL Mailchimp Members] ソース接続の作成

このチュートリアルでは、ユーザーインターフェイスを使用して [!DNL Mailchimp] ソースコネクタを作成し、[!DNL Mailchimp Members] データを Adobe Experience Platform に取り込む手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## 必要な資格情報の収集

[!DNL Mailchimp Members] データをExperience Platformに取り込むには、まず、[!DNL Mailchimp] アカウントに対応する適切な認証情報を提供する必要があります。

[!DNL Mailchimp Members] ソースは、OAuth 2 更新コードと基本認証の両方をサポートしています。 これらの認証タイプの詳細については、次の表を参照してください。　

### OAuth 2 更新コード

| 資格情報 | 説明 |
| --- | --- |
| ドメイン | MailChimp API への接続に使用するルート URL です。 ルート URL の形式は `https://{DC}.api.mailchimp.com` で、 `{DC}` はお使いのアカウントに対応するデータセンターを表します。 |
| 認証テスト URL | 認証テスト URL は、Experience Platformに接続する際に認証情報を検証 [!DNL Mailchimp] るために使用されます。 これを指定しない場合、代わりにソース接続の作成手順の間に資格情報が自動的にチェックされます。 |
| アクセストークン | ソースの認証に使用された、対応するアクセストークン。これは、OAuth ベースの認証に必要です。 |

Oauth 2 を使用したExperience Platformへの [!DNL Mailchimp] アカウント認証の詳細については、こちらの [[!DNL Mailchimp] OAuth 2 の使用に関するドキュメント ](https://mailchimp.com/developer/marketing/guides/access-user-data-oauth-2/) を参照してください。

### 基本認証

| 資格情報 | 説明 |
| --- | --- |
| ドメイン | MailChimp API への接続に使用するルート URL です。 ルート URL の形式は `https://{DC}.api.mailchimp.com` で、`{DC}` はお使いのアカウントに対応するデータセンターを表します。 |
| ユーザー名 | MailChimp アカウントに対応するユーザー名。 これは、基本認証に必要です。 |
| パスワード | MailChimp アカウントに対応するパスワードです。 これは、基本認証に必要です。 |

## [!DNL Mailchimp Members] アカウントのExperience Platformへの接続

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL  ソース ] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL マーケティング自動化]のカテゴリで、「**[!UICONTROL Mailchimp キャンペーン]**」、「**[!UICONTROL データを追加]**」の順に選択します。

![カタログ](../../../../images/tutorials/create/mailchimp-members/catalog.png)

**[!UICONTROL Mailchimp キャンペーンアカウントの接続]**&#x200B;ページが表示されます。 このページでは、既存のアカウントにアクセスするか、新しいアカウントを作成するかを選択することができます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Mailchimp Members] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/mailchimp-members/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、[!DNL Mailchimp Members]ソース接続の詳細の名前と説明を入力します。

![新規](../../../../images/tutorials/create/mailchimp-members/new.png)


#### OAuth 2 を使用した認証

OAuth 2 を使用するには、「[!UICONTROL OAuth 2 更新コード ]」を選択し、ドメイン、認証テスト URL、アクセストークンの値を入力して「**[!UICONTROL ソースに接続]**」を選択します。 資格情報が検証されるまでしばらく待ってから、「**[!UICONTROL 次へ]**」を選択して続行します。

![oauth](../../../../images/tutorials/create/mailchimp-members/oauth.png)

#### 基本認証を使用した認証

基本認証を使用する場合は、「[!UICONTROL  基本認証 ]」を選択し、ドメイン、ユーザー名、パスワードに値を入力して「**[!UICONTROL ソースに接続]**」を選択します。 資格情報が検証されるまでしばらく待ってから、「**[!UICONTROL 次へ]**」をクリックして続行します。

![基本](../../../../images/tutorials/create/mailchimp-members/basic.png)

### [!DNL Mailchimp Members] データを選択

ソースが認証されたら、[!DNL Mailchimp Members] アカウントに対応する `listId` を提供する必要があります。

[!UICONTROL データを選択]ページで`listId` を入力して「**[!UICONTROL 参照]**」を選択します。

![参照](../../../../images/tutorials/create/mailchimp-members/explore.png)

ページが更新されてインタラクティブスキーマツリーに変わり、データの階層を調べることができます。「**[!UICONTROL 次へ]**」を選択して次に進みます。

![select-data](../../../../images/tutorials/create/mailchimp-members/select-data.png)

## 次の手順

[!DNL Mailchimp] アカウントが認証され、[!DNL Mailchimp Members] データが選択されたので、データフローの作成を開始して、データをExperience Platformに取り込むことができます。 データフローの作成方法に関する詳細な手順については、[ マーケティング自動化データをExperience Platformに取り込むためのデータフローの作成 ](../../dataflow/marketing-automation.md) のドキュメントを参照してください。
