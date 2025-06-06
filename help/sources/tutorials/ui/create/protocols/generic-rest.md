---
keywords: Experience Platform；ホーム；人気のトピック；汎用 REST API
title: UI での汎用 REST API Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、汎用の REST API ソース接続を作成する方法を説明します。
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 23%

---

# UI での [!DNL Generic REST API] ソース接続の作成

>[!NOTE]
>
> [!DNL Generic REST API] ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL Generic REST API] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な資格情報の収集

Experience Platformで [!DNL Generic REST API] アカウントにアクセスするには、選択した認証タイプの有効な資格情報を指定する必要があります。 汎用の REST API は、OAuth 2 更新コードと基本認証の両方をサポートしています。 次の表に、サポートされる 2 つの認証タイプの資格情報を示します。

#### OAuth 2 更新コード

| 資格情報 | 説明 |
| --- | --- |
| ホスト | リクエスト先のソースのホスト URL。 この値は必須であり、リクエストパラメーターの上書きを使用してバイパスすることはできません。 |
| 認証テスト URL | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用されます。指定しない場合、代わりにソース接続の作成時に資格情報が自動的にチェックされます。 |
| クライアント ID | （オプション）ユーザーアカウントに関連付けられたクライアント ID。 |
| クライアントシークレット | （オプション）ユーザーアカウントに関連付けられたクライアントの秘密鍵。 |
| アクセストークン | アプリケーションへのアクセスに使用するプライマリ認証情報。 アクセストークンは、ユーザーのデータの特定の側面にアクセスするための、アプリケーションの認証を表します。 この値は必須であり、リクエストパラメーターの上書きを使用してバイパスすることはできません。 |
| 更新トークン | （オプション）アクセストークンの有効期限が切れた場合に、新しいアクセストークンの生成に使用されるトークン。 |
| アクセストークン URL | （任意）アクセストークンの取得に使用する URL エンドポイント。 |
| リクエストパラメーターの上書き | （オプション）上書きする資格情報パラメーターを指定できるプロパティ。 |


#### 基本認証

| 資格情報 | 説明 |
| --- | --- |
| ホスト | リクエスト先のソースのホスト URL。 |
| ユーザー名 | ユーザーアカウントに対応するユーザー名。 |
| パスワード | ユーザーアカウントに対応するパスワード。 |

## 汎用 REST API アカウントの接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

[!UICONTROL &#x200B; プロトコル &#x200B;] カテゴリで、「**[!UICONTROL 汎用 REST API]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/generic-rest/catalog.png)

**[!UICONTROL 汎用 REST API に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する汎用 REST API アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/generic-rest/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL Generic REST API] アカウントの名前とオプションの説明を入力します。

![新規](../../../../images/tutorials/create/generic-rest/new.png)

#### OAuth 2 更新コードを使用した認証

[!DNL Generic REST API] は、OAuth 2 更新コードと基本認証の両方をサポートしています。 OAuth2 認証を使用して認証するには、「**[!UICONTROL OAuth2RefreshCode]**」を選択し、OAuth 2 資格情報を入力して「**[!UICONTROL ソースに接続]**」を選択します。

![](../../../../images/tutorials/create/generic-rest/oauth2.png)

#### 基本認証を使用した認証

基本認証を使用する場合は、「**[!UICONTROL 基本認証]**」を選択し、ホスト、ユーザー名、パスワードを入力して「**[!UICONTROL ソースに接続]**」を選択します。

![](../../../../images/tutorials/create/generic-rest/basic-authentication.png)

## 次の手順

このチュートリアルでは、汎用 REST API アカウントとの接続を確立しました。 次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/protocols.md) を行いましょう。
