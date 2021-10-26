---
keywords: Experience Platform；ホーム；人気のあるトピック；汎用 REST API
title: UI での汎用 REST API ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して汎用 REST API ソース接続を作成する方法を説明します。
source-git-commit: 94809a8e98c8de7a9a474fb5543b590fc51cb075
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 8%

---

# の作成 [!DNL Generic REST API] UI でのソース接続

>[!NOTE]
>
> 10. [!DNL Generic REST API] ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベルのコネクタの使用に関する詳細

このチュートリアルでは、 [!DNL Generic REST API] Adobe Experience Platformユーザーインターフェイスを使用したソースコネクタ

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必要な資格情報の収集

を使用して、 [!DNL Generic REST API] Platform のアカウントで、選択した認証タイプに有効な資格情報を指定する必要があります。 汎用 REST API は、OAuth 2 更新コードと基本認証の両方をサポートしています。 次の表に、サポートされる 2 つの認証タイプの資格情報を示します。

#### OAuth 2 更新コード

| 資格情報 | 説明 |
| --- | --- |
| ホスト | リクエスト先のソースのホスト URL。 この値は必須で、リクエストパラメーターの上書きを使用してバイパスできません。 |
| 認証テスト URL | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用します。 指定しない場合、代わりに、ソース接続の作成手順で資格情報が自動的にチェックされます。 |
| クライアント ID | （オプション）ユーザーアカウントに関連付けられているクライアント ID。 |
| クライアント秘密鍵 | （オプション）ユーザーアカウントに関連付けられたクライアント秘密鍵。 |
| アクセストークン | アプリケーションへのアクセスに使用するプライマリ認証資格情報。 アクセストークンは、ユーザーのデータの特定の側面にアクセスするための、アプリケーションの認証を表します。 この値は必須で、リクエストパラメーターの上書きを使用してバイパスできません。 |
| 更新トークン | （オプション）アクセストークンの有効期限が切れたときに、新しいアクセストークンの生成に使用されるトークン。 |
| アクセストークン URL | （オプション）アクセストークンの取得に使用する URL エンドポイント。 |
| リクエストパラメーターの上書き | （オプション）上書きする秘密鍵証明書のパラメーターを指定できるプロパティ。 |


#### 基本認証

| 資格情報 | 説明 |
| --- | --- |
| ホスト | リクエスト先のソースのホスト URL。 |
| ユーザー名 | ユーザーアカウントに対応するユーザー名。 |
| パスワード | ユーザーアカウントに対応するパスワード。 |

## 汎用 REST API アカウントの接続

プラットフォーム UI で、「 **[!UICONTROL ソース]** 左側のナビゲーションから、 [!UICONTROL ソース] ワークスペース。 10. [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

の [!UICONTROL プロトコル] カテゴリ，選択 **[!UICONTROL 汎用 REST API]** 次に、「 **[!UICONTROL データの追加]**.

![カタログ](../../../../images/tutorials/create/generic-rest/catalog.png)

10. **[!UICONTROL 汎用 REST API への接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する汎用 REST API アカウントを選択し、「 」を選択します **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/generic-rest/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新規アカウント]**&#x200B;新しい [!DNL Generic REST API] アカウント

![新規](../../../../images/tutorials/create/generic-rest/new.png)

#### OAuth 2 更新コードを使用した認証

[!DNL Generic REST API] は、OAuth 2 更新コードと基本認証の両方をサポートしています。 OAuth2 認証を使用して認証するには、「 **[!UICONTROL OAuth2RefreshCode]**」をクリックし、OAuth 2 資格情報を入力して、「 **[!UICONTROL ソースに接続]**.

![](../../../../images/tutorials/create/generic-rest/oauth2.png)

#### 基本認証を使用した認証

基本認証を使用する場合は、 **[!UICONTROL 基本認証]**、ホスト、ユーザー名およびパスワードを入力して、「 」を選択します。 **[!UICONTROL ソースに接続]**.

![](../../../../images/tutorials/create/generic-rest/basic-authentication.png)

## 次の手順

このチュートリアルに従って、汎用 REST API アカウントへの接続を確立しました。 次のチュートリアルに進み、 [データを Platform に取り込むためのデータフローの設定](../../dataflow/protocols.md).
