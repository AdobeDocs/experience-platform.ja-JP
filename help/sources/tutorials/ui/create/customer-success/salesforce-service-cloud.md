---
title: Experience Platformユーザーインターフェイスを使用した Salesforce サービスクラウドアカウントの接続
description: ユーザーインターフェイスを使用して Salesforce Service Cloud アカウントを連携し、カスタマーサクセスデータをExperience Platformに取り込む方法について説明します。
exl-id: 38480a29-7852-46c6-bcea-5dc6bffdbd15
source-git-commit: 7930a869627130a5db34780e64b809cda0c1e5f4
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 21%

---

# を接続 [!DNL Salesforce Service Cloud] ui を使用したExperience Platformへのアカウント設定

このチュートリアルでは、を接続する手順を説明します [!DNL Salesforce Service Cloud] Experience Platformユーザーインターフェイスを使用して、アカウントを設定し、カスタマーサクセスデータをAdobe Experience Platformに取り込みます。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効ながある場合 [!DNL Salesforce Service Cloud] 接続する場合は、このドキュメントの残りの部分をスキップし、に関するチュートリアルに進んでください。 [カスタマーサクセスのためのデータフローの設定](../../dataflow/customer-success.md)

### 必要な資格情報の収集

この [!DNL Salesforce Service Cloud] ソースは、基本認証および OAuth2 クライアント資格情報をサポートします。

>[!BEGINTABS]

>[!TAB 基本認証]

を接続するには、次の資格情報の値を指定する必要があります [!DNL Salesforce Service Cloud] 基本認証を使用するアカウント。

| 資格情報 | 説明 |
| --- | --- |
| 環境 URL | の URL [!DNL Salesforce Service Cloud] ソースインスタンス。 |
| ユーザー名 | のユーザー名 [!DNL Salesforce Service Cloud] ユーザーアカウント。 |
| パスワード | パスワード： [!DNL Salesforce Service Cloud] ユーザーアカウント。 |
| セキュリティトークン | のセキュリティトークン [!DNL Salesforce Service Cloud] ユーザーアカウント。 |
| API バージョン | （オプション）の REST API バージョン [!DNL Salesforce Service Cloud] 使用しているインスタンス。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョンを使用している場合 `52`の場合、値をに入力する必要があります `52.0`. このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |

認証について詳しくは、次を参照してください [この [!DNL Salesforce Service Cloud] 認証ガイド](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart_oauth.htm).

>[!TAB OAuth2 クライアント資格情報]

を接続するには、次の資格情報の値を指定する必要があります [!DNL Salesforce Service Cloud] oauth2 クライアント資格情報を使用するアカウント。

| 資格情報 | 説明 |
| --- | --- |
| 環境 URL | の URL [!DNL Salesforce Service Cloud] ソースインスタンス。 |
| クライアント ID | クライアント ID は、OAuth2 認証の一部として、クライアント秘密鍵と並行して使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、アプリケーションを識別して、アカウントに代わってアプリケーションを動作させることができます [!DNL Salesforce Service Cloud]. |
| クライアントシークレット | クライアントの秘密鍵は、クライアント ID と並行して、OAuth2 認証の一部として使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、アプリケーションを識別して、アカウントに代わってアプリケーションを動作させることができます [!DNL Salesforce Service Cloud]. |
| API バージョン | の REST API バージョン [!DNL Salesforce Service Cloud] 使用しているインスタンス。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョンを使用している場合 `52`の場合、値をに入力する必要があります `52.0`. このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |

用の OAuth の使用の詳細 [!DNL Salesforce Service Cloud]、を読み取ります [[!DNL Salesforce Service Cloud] oauth 認証フローのガイド](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&amp;type=5).

>[!ENDTABS]

必要な資格情報を収集したら、次の手順に従って接続できます [!DNL Salesforce Service Cloud] Experience Platformするアカウント。

## [!DNL Salesforce Service Cloud] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

を選択 **[!DNL Salesforce Service Cloud]** の下 *[!UICONTROL カスタマーサクセス]* カテゴリを選択してから、 **[!UICONTROL データを追加]**.

>[!TIP]
>
>ソースカタログ内のソースには、 **[!UICONTROL の設定]** 特定のソースがまだ認証済みアカウントを持っていない場合のオプション。 認証済みアカウントが存在すると、このオプションはに変更されます。 **[!UICONTROL データを追加]**.

![Salesforce Service Cloud ソースカードが選択されたExperience PlatformUI のソースカタログ。](../../../../images/tutorials/create/salesforce-service-cloud/catalog.png)

この **[!UICONTROL Salesforce Service Cloud への接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウントを使用

既存のアカウントを使用するには、を選択します **[!UICONTROL 既存のアカウント]**&#x200B;表示されるリストから目的のアカウントを選択します。 終了したら、 **[!UICONTROL 次]** をクリックして続行します。

![組織に既に存在する、認証済みの Salesforce Service クラウドアカウントのリスト。](../../../../images/tutorials/create/salesforce-service-cloud/existing.png)

### 新しいアカウントを作成

新しいアカウントを作成するには、を選択します **[!UICONTROL 新しいアカウント]** 新しいの名前と説明を入力します [!DNL Salesforce Service Cloud] アカウント。

![適切な認証資格情報を提供することで、新しい Salesforce サービスクラウドアカウントを作成するためのインターフェイス。](../../../../images/tutorials/create/salesforce-service-cloud/new.png)

次に、新しいアカウントに使用する認証タイプを選択します。

>[!BEGINTABS]

>[!TAB 基本認証]

基本認証の場合、次を選択します **[!UICONTROL 基本認証]** 次に、次の資格情報の値を指定します。

* 環境 URL
* ユーザー名
* パスワード
* API バージョン （オプション）

終了したら、 **[!UICONTROL ソースに接続]**.

![Salesforce アカウント作成用の基本認証インターフェイス。](../../../../images/tutorials/create/salesforce-service-cloud/basic.png)

>[!TAB OAuth2 クライアント資格情報]

OAuth 2 クライアント資格情報の場合は、を選択します。 **[!UICONTROL OAuth2 クライアント資格情報]** 次に、次の資格情報の値を指定します。

* 環境 URL
* クライアント ID
* クライアントシークレット
* API バージョン

終了したら、 **[!UICONTROL ソースに接続]**.

![Salesforce アカウント作成用の OAuth インターフェイス。](../../../../images/tutorials/create/salesforce-service-cloud/oauth2.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Salesforce Service Cloud] アカウントとの接続を確立しました。次のチュートリアルに進むことができます。 [カスタマーサクセスデータをExperience Platformに取り込むためのデータフローの設定](../../dataflow/customer-success.md).
