---
title: Experience Platformユーザーインターフェイスを使用した Salesforce アカウントの接続
description: ユーザーインターフェイスを使用して Salesforce アカウントを連携し、CRM データをExperience Platformに取り込む方法について説明します。
exl-id: b67fa4c4-d8ff-4d2d-aa76-5d9d32aa22d6
source-git-commit: 8d62cf4ca0071e84baa9399e0a25f7ebfb096c1a
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 21%

---

# を接続 [!DNL Salesforce] ui を使用したExperience Platformへのアカウント設定

このチュートリアルでは、を接続する手順を説明します [!DNL Salesforce] Experience Platformユーザーインターフェイスを使用して、アカウントを設定し、CRM データをAdobe Experience Platformに取り込みます。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に認証済みの場合 [!DNL Salesforce] アカウントの場合は、このドキュメントの残りの部分をスキップし、に関するチュートリアルに進むことができます。 [crm データのデータフローの設定](../../dataflow/crm.md).

### 必要な資格情報の収集 {#gather-required-credentials}

この [!DNL Salesforce] ソースは、基本認証および OAuth2 クライアント資格情報をサポートします。

>[!BEGINTABS]

>[!TAB 基本認証]

を接続するには、次の資格情報の値を指定する必要があります [!DNL Salesforce] 基本認証を使用するアカウント。

| 資格情報 | 説明 |
| --- | --- |
| 環境 URL | の URL [!DNL Salesforce] ソースインスタンス。 |
| ユーザー名 | のユーザー名 [!DNL Salesforce] ユーザーアカウント。 |
| パスワード | パスワード： [!DNL Salesforce] ユーザーアカウント。 |
| セキュリティトークン | のセキュリティトークン [!DNL Salesforce] ユーザーアカウント。 |
| API バージョン | （オプション）の REST API バージョン [!DNL Salesforce] 使用しているインスタンス。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョンを使用している場合 `52`の場合、値をに入力する必要があります `52.0`. このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |

認証について詳しくは、次を参照してください [この [!DNL Salesforce] 認証ガイド](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart_oauth.htm).

>[!TAB OAuth2 クライアント資格情報]

を接続するには、次の資格情報の値を指定する必要があります [!DNL Salesforce] oauth2 クライアント資格情報を使用するアカウント。

| 資格情報 | 説明 |
| --- | --- |
| 環境 URL | の URL [!DNL Salesforce] ソースインスタンス。 |
| クライアント ID | クライアント ID は、OAuth2 認証の一部として、クライアント秘密鍵と並行して使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、アプリケーションを識別して、アカウントに代わってアプリケーションを動作させることができます [!DNL Salesforce]. |
| クライアントシークレット | クライアントの秘密鍵は、クライアント ID と並行して、OAuth2 認証の一部として使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、アプリケーションを識別して、アカウントに代わってアプリケーションを動作させることができます [!DNL Salesforce]. |
| API バージョン | の REST API バージョン [!DNL Salesforce] 使用しているインスタンス。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョンを使用している場合 `52`の場合、値をに入力する必要があります `52.0`. このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |

用の OAuth の使用の詳細 [!DNL Salesforce]、を読み取ります [[!DNL Salesforce] oauth 認証フローのガイド](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&amp;type=5).

>[!ENDTABS]

必要な資格情報を収集したら、次の手順に従って接続できます [!DNL Salesforce] Experience Platformするアカウント。

## [!DNL Salesforce] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 *CRM* カテゴリ、選択 **[!DNL Salesforce]**&#x200B;を選択してから、 **[!UICONTROL データを追加]**.

>[!TIP]
>
>ソースカタログ内のソースには、 **[!UICONTROL の設定]** 特定のソースがまだ認証済みアカウントを持っていない場合のオプション。 認証済みアカウントが存在すると、このオプションはに変更されます。 **[!UICONTROL データを追加]**.

![Salesforce ソースカードが選択されたExperience PlatformUI のソースカタログ。](../../../../images/tutorials/create/salesforce/catalog.png)

この **[!UICONTROL Salesforce への接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウントを使用

既存のアカウントを使用するには、を選択します **[!UICONTROL 既存のアカウント]** 次に、表示されるリストから使用するアカウントを選択します。 終了したら、 **[!UICONTROL 次]** をクリックして続行します。

![組織に既に存在する、認証済みの Salesforce アカウントのリスト。](../../../../images/tutorials/create/salesforce/existing.png)

### 新しいアカウントを作成

新しいアカウントを作成するには、を選択します **[!UICONTROL 新しいアカウント]** 新しいの名前と説明を入力します [!DNL Salesforce] アカウント。

![適切な認証資格情報を提供することで、新しい Salesforce アカウントを作成できるインターフェイス。](../../../../images/tutorials/create/salesforce/new.png)

次に、新しいアカウントに使用する認証タイプを選択します。

>[!BEGINTABS]

>[!TAB 基本認証]

基本認証の場合、次を選択します **[!UICONTROL 基本認証]** 次に、次の資格情報の値を指定します。

* 環境 URL
* ユーザー名
* パスワード
* API バージョン （オプション）

終了したら、 **[!UICONTROL ソースに接続]**.

![Salesforce アカウント作成用の基本認証インターフェイス。](../../../../images/tutorials/create/salesforce/basic.png)

>[!TAB OAuth2 クライアント資格情報]

OAuth 2 クライアント資格情報の場合は、を選択します。 **[!UICONTROL OAuth2 クライアント資格情報]** 次に、次の資格情報の値を指定します。

* 環境 URL
* クライアント ID
* クライアントシークレット
* API バージョン

終了したら、 **[!UICONTROL ソースに接続]**.

![Salesforce アカウント作成用の OAuth インターフェイス。](../../../../images/tutorials/create/salesforce/oauth2.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Salesforce] アカウントとの接続を確立しました。次のチュートリアルに進むことができます。 [データをに取り込むデータフローの設定 [!DNL Platform]](../../dataflow/crm.md).
