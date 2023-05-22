---
title: UI を使用して SalesforceMarketing CloudアカウントをExperience Platformに接続
description: UI を使用して SalesforceMarketing CloudアカウントをExperience Platformに接続する方法を説明します。
exl-id: 1d9bde60-31e0-489c-9c1c-b6471e0ea554
source-git-commit: 997a9dc70145a8cfd5d6da20ba788a4610e5c257
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 27%

---

# 接続 [!DNL Salesforce Marketing Cloud] UI を通じてExperience Platformにアカウント

>[!IMPORTANT]
>
>カスタムオブジェクトの取り込みは、現在、 [!DNL Salesforce Marketing Cloud] ソースの統合。


このチュートリアルでは、 [!DNL Salesforce Marketing Cloud] UI 経由でAdobe Experience Platformにアカウントします。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL Salesforce Marketing Cloud] アカウントを使用する場合は、このドキュメントの残りの部分をスキップし、次のチュートリアルに進んでください： [UI を使用したマーケティング自動化データのExperience Platformへの取り込み](../../dataflow/marketing-automation.md).

### 必要な認証情報の収集

Platform で [!DNL Salesforce Marketing Cloud] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| ホスト | アプリケーションのホストサーバー。 多くの場合、これはサブドメインです。 **注意：** を `host` の値を指定する場合、URL 全体ではなくサブドメインのみを指定する必要があります。 例えば、ホスト URL が `https://abcd-ab12c3d4e5fg6hijk7lmnop8qrst.auth.marketingcloudapis.com/`を指定した場合は、 `abcd-ab12c3d4e5fg6hijk7lmnop8qrst` をホスト値として使用します。 |
| クライアント ID | 次に関連付けられたクライアント ID: [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| クライアントシークレット | に関連付けられたクライアント秘密鍵 [!DNL Salesforce Marketing Cloud] アプリケーション。 |

の認証の詳細 [!DNL Salesforce Marketing Cloud]、 [[!DNL Salesforce] 認証ドキュメント](https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/authentication.htm).

## [!DNL Salesforce Marketing Cloud] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。この [!UICONTROL カタログ] は、Experience Platformでサポートされる様々なソースを表示します。

カテゴリのリストから適切なカテゴリを選択できます。 また、検索バーを使用して、特定のソースをフィルタリングすることもできます。

以下 [!UICONTROL マーケティングの自動化] カテゴリ、選択 **[!UICONTROL SalesforceMarketing Cloud]** 次に、 **[!UICONTROL 設定]**.

![SalesforceMarketing Cloudソースが選択されたソースカタログ](../../../../images/tutorials/create/salesforce-marketing-cloud/catalog.png)

この **[!UICONTROL SalesforceMarketing Cloudに接続]** ページが表示されます。 このページでは、新しいアカウントを作成するか、既存のアカウントを使用できます。

### 新規アカウント

新しいアカウントを作成するには、 **[!UICONTROL 新しいアカウント]** アカウントの名前、オプションの説明、および [!DNL Salesforce Marketing Cloud] アカウント

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![SalesforceMarketing Cloudの新しいアカウントを認証できる新しいアカウントインターフェイス。](../../../../images/tutorials/create/salesforce-marketing-cloud/new.png)

### 既存のアカウント

既に既存のアカウントがある場合は、「 **[!UICONTROL 既存のアカウント]** 次に、表示されるリストから、使用するアカウントを選択します。

![既存の Salesforce アカウントのリストから選択できる既存のMarketing Cloudインターフェイス。](../../../../images/tutorials/create/salesforce-marketing-cloud/existing.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Salesforce Marketing Cloud] アカウントとExperience Platform。 次のチュートリアルに進み、 [データフローを作成して、マーケティング自動化データをExperience Platformに取り込む](../../dataflow/marketing-automation.md).
