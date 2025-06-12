---
title: Experience Platform ユーザーインターフェイスを使用してSalesforce Service Cloud アカウントに接続します
description: ユーザーインターフェイスを使用してSalesforce Service Cloud アカウントを接続し、カスタマーサクセスデータをExperience Platformに取り込む方法について説明します。
exl-id: 38480a29-7852-46c6-bcea-5dc6bffdbd15
source-git-commit: eab6303a3b420d4622185316922d242a4ce8a12d
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 18%

---

# UI を使用した [!DNL Salesforce Service Cloud] アカウントのExperience Platformへの接続

このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用して [!DNL Salesforce Service Cloud] アカウントを連携し、カスタマーサクセスデータをAdobe Experience Platformに取り込む手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Salesforce Service Cloud] 接続がある場合は、このドキュメントの残りの部分をスキップし、[ カスタマーサクセスのためのデータフローの設定 ](../../dataflow/customer-success.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

>[!WARNING]
>
>[!DNL Salesforce Service Cloud] ソースの基本認証は、2026 年 1 月に廃止されます。 ソースの使用と [!DNL Salesforce Service Cloud] アカウントからExperience Platformへのデータの取り込みを続行するには、OAuth 2 クライアント資格情報認証に移行する必要があります。

[!DNL Salesforce Service Cloud] ソースは、基本認証と OAuth2 クライアント資格情報をサポートしています。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を使用して [!DNL Salesforce Service Cloud] アカウントに接続するには、次の資格情報の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| 環境 URL | [!DNL Salesforce Service Cloud] ソースインスタンスの URL。 |
| ユーザー名 | [!DNL Salesforce Service Cloud] ユーザーアカウントのユーザー名。 |
| パスワード | [!DNL Salesforce Service Cloud] ユーザーアカウントのパスワード。 |
| セキュリティトークン | [!DNL Salesforce Service Cloud] ユーザーアカウントのセキュリティ トークン。 |
| API バージョン | （オプション）使用している [!DNL Salesforce Service Cloud] インスタンスの REST API バージョン。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョン `52` を使用している場合、値を `52.0` と入力する必要があります。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新バージョンが自動的に使用されます。 |

認証について詳しくは、[ この  [!DNL Salesforce Service Cloud]  認証ガイド ](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart_oauth.htm) を参照してください。

>[!TAB OAuth2 クライアント資格情報 ]

OAuth2 クライアント資格情報を使用して [!DNL Salesforce Service Cloud] アカウントに接続するには、次の資格情報の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| 環境 URL | [!DNL Salesforce Service Cloud] ソースインスタンスの URL。 |
| クライアント ID | クライアント ID は、OAuth2 認証の一部として、クライアント秘密鍵と並行して使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、[!DNL Salesforce Service Cloud] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションが動作することができます。 |
| クライアントシークレット | クライアントの秘密鍵は、クライアント ID と並行して、OAuth2 認証の一部として使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、[!DNL Salesforce Service Cloud] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションが動作することができます。 |
| API バージョン | 使用している [!DNL Salesforce Service Cloud] インスタンスの REST API バージョン。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョン `52` を使用している場合、値を `52.0` と入力する必要があります。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新バージョンが自動的に使用されます。 |

[!DNL Salesforce Service Cloud] に対する OAuth の使用について詳しくは、[[!DNL Salesforce Service Cloud] OAuth 認証フローのガイド ](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&type=5) を参照してください。

>[!ENDTABS]

必要な資格情報を収集したら、次の手順に従って [!DNL Salesforce Service Cloud] アカウントをExperience Platformに接続できます。

## [!DNL Salesforce Service Cloud] アカウントを接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*[!UICONTROL カスタマーサクセス]* カテゴリの下の **[!DNL Salesforce Service Cloud]** を選択してから、**[!UICONTROL データを追加]** を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントが存在すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![Experience Platform UI のソースカタログで、Salesforce Service Cloud ソースカードが選択されています。](../../../../images/tutorials/create/salesforce-service-cloud/catalog.png)

**[!UICONTROL Salesforce Service Cloud への接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウントを使用

既存のアカウントを使用するには、**[!UICONTROL 既存のアカウント]** を選択し、表示されるリストから目的のアカウントを選択します。 終了したら、「**[!UICONTROL 次へ]** を選択して続行します。

![ 組織内に既に存在する、認証済みのSalesforce Service Cloud アカウントのリスト。](../../../../images/tutorials/create/salesforce-service-cloud/existing.png)

### 新しいアカウントを作成

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL Salesforce Service Cloud] アカウントの名前と説明を入力します。

![ 適切な認証資格情報を提供することで、新しいSalesforce Service Cloud アカウントを作成できるインターフェイス。](../../../../images/tutorials/create/salesforce-service-cloud/new.png)

次に、新しいアカウントに使用する認証タイプを選択します。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証の場合は、「**[!UICONTROL 基本認証]** を選択し、次の資格情報の値を指定します。

* 環境 URL
* ユーザー名
* パスワード
* API バージョン （オプション）

終了したら「**[!UICONTROL ソースに接続]**」を選択します。

![Salesforce アカウント作成用の基本認証インターフェイス ](../../../../images/tutorials/create/salesforce-service-cloud/basic.png)

>[!TAB OAuth2 クライアント資格情報 ]

「OAuth 2 クライアント資格情報」で、「**[!UICONTROL OAuth2 クライアント資格情報]** を選択し、次の資格情報の値を入力します。

* 環境 URL
* クライアント ID
* クライアントシークレット
* API バージョン

終了したら「**[!UICONTROL ソースに接続]**」を選択します。

![Salesforce アカウント作成用の OAuth インターフェイス ](../../../../images/tutorials/create/salesforce-service-cloud/oauth2.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Salesforce Service Cloud] アカウントとの接続を確立しました。次のチュートリアルに進み、[ カスタマーサクセスデータをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/customer-success.md) を行いましょう。
