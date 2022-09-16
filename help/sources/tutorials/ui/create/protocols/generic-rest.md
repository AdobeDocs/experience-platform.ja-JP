---
keywords: Experience Platform；ホーム；人気の高いトピック；汎用 REST API
title: UI での汎用 REST API ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して汎用 REST API ソース接続を作成する方法を説明します。
source-git-commit: 94809a8e98c8de7a9a474fb5543b590fc51cb075
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 35%

---

# UI での [!DNL Generic REST API] ソース接続の作成

>[!NOTE]
>
> この [!DNL Generic REST API] ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

このチュートリアルでは、 [!DNL Generic REST API] Adobe Experience Platformユーザーインターフェイスを使用したソースコネクタ

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な認証情報の収集

次の項目にアクセスするには、 [!DNL Generic REST API] Platform のアカウントで、選択した認証タイプに有効な資格情報を指定する必要があります。 汎用 REST API は、OAuth 2 更新コードと基本認証の両方をサポートしています。 サポートされる 2 つの認証タイプの資格情報については、次の表を参照してください。

#### OAuth 2 更新コード

| 認証情報 | 説明 |
| --- | --- |
| ホスト | リクエスト先のソースのホスト URL。 この値は必須で、リクエストパラメーターの上書きを使用してバイパスすることはできません。 |
| 認証テスト URL | （オプション）認証テスト URL は、ベース接続の作成時に認証情報を検証するために使用されます。指定しない場合、代わりにソース接続の作成時に資格情報が自動的にチェックされます。 |
| クライアント ID | （オプション）ユーザーアカウントに関連付けられたクライアント ID。 |
| クライアント秘密鍵 | （オプション）ユーザーアカウントに関連付けられたクライアント秘密鍵。 |
| アクセストークン | アプリケーションへのアクセスに使用するプライマリ認証資格情報。 アクセストークンは、ユーザーのデータの特定の側面にアクセスするための、アプリケーションの認証を表します。 この値は必須で、リクエストパラメーターの上書きを使用してバイパスすることはできません。 |
| 更新トークン | （オプション）アクセストークンの有効期限が切れたときに、新しいアクセストークンの生成に使用されるトークン。 |
| トークン URL にアクセス | （オプション）アクセストークンの取得に使用する URL エンドポイント。 |
| リクエストパラメーターの上書き | （オプション）上書きする秘密鍵証明書のパラメーターを指定できるプロパティ。 |


#### 基本認証

| 認証情報 | 説明 |
| --- | --- |
| ホスト | リクエスト先のソースのホスト URL。 |
| ユーザー名 | ユーザーアカウントに対応するユーザー名。 |
| パスワード | ユーザーアカウントに対応するパスワード。 |

## 汎用 REST API アカウントの接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

以下 [!UICONTROL プロトコル] カテゴリ、選択 **[!UICONTROL 汎用 REST API]** 次に、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/generic-rest/catalog.png)

この **[!UICONTROL 汎用 REST API への接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する汎用 REST API アカウントを選択し、「 」を選択します **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/generic-rest/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;新しい [!DNL Generic REST API] アカウント

![新規](../../../../images/tutorials/create/generic-rest/new.png)

#### OAuth 2 を使用した認証 コードを更新

[!DNL Generic REST API] は、OAuth 2 更新コードと基本認証の両方をサポートしています。 OAuth2 認証を使用して認証するには、「 」を選択します。 **[!UICONTROL OAuth2RefreshCode]**&#x200B;をクリックし、OAuth 2 資格情報を入力して、 **[!UICONTROL ソースに接続]**.

![](../../../../images/tutorials/create/generic-rest/oauth2.png)

#### 基本認証を使用した認証

基本認証を使用する場合は、 **[!UICONTROL 基本認証]**、ホスト、ユーザー名およびパスワードを入力し、「 」を選択します。 **[!UICONTROL ソースに接続]**.

![](../../../../images/tutorials/create/generic-rest/basic-authentication.png)

## 次の手順

このチュートリアルに従って、Generic REST API アカウントへの接続を確立しました。 次のチュートリアルに進み、[データを Platform に取り込むためのデータフローの設定](../../dataflow/protocols.md)を行いましょう。
