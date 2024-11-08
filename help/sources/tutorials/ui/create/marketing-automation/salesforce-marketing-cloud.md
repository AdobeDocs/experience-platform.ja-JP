---
title: SalesforceMarketing Cloudアカウントを UI を使用してExperience Platformに接続する
description: UI を通じてSalesforceMarketing CloudアカウントをExperience Platformに接続する方法を説明します。
exl-id: 1d9bde60-31e0-489c-9c1c-b6471e0ea554
source-git-commit: 474b81aa8caf58013f8ea7cff9ad59d92466aac8
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 27%

---

# UI を使用して [!DNL Salesforce Marketing Cloud] アカウントをExperience Platformに接続する

>[!WARNING]
>
>[!DNL Salesforce Marketing Cloud] ソースは 2025 年 5 月末に非推奨（廃止予定）になります。

このチュートリアルでは、UI を通じて [!DNL Salesforce Marketing Cloud] アカウントをAdobe Experience Platformに接続する方法の手順を説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL Salesforce Marketing Cloud] アカウントを持っている場合は、このドキュメントの残りの部分をスキップし、[UI を使用したマーケティング自動化データのExperience Platformへの取り込み ](../../dataflow/marketing-automation.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

Platform で [!DNL Salesforce Marketing Cloud] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| ホスト | アプリケーションのホストサーバー。 多くの場合、これはサブドメインです。 **メモ：**`host` 値を入力する場合は、`{subdomain}.rest.marketingcloudapis.com` を指定する必要があります。 例えば、ホスト URL が `https://acme-ab12c3d4e5fg6hijk7lmnop8qrst.auth.marketingcloudapis.com/` の場合、ホスト値として `acme-ab12c3d4e5fg6hijk7lmnop8qrst.rest.marketingcloudapis.com/` を入力する必要があります。 |
| クライアント ID | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント ID。 |
| クライアントシークレット | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント秘密鍵。 |

[!DNL Salesforce Marketing Cloud] の認証について詳しくは、[[!DNL Salesforce]  認証ドキュメント ](https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/authentication.htm) を参照してください。

## [!DNL Salesforce Marketing Cloud] アカウントを接続

>[!IMPORTANT]
>
>カスタムオブジェクトの取り込みは、現在、[!DNL Salesforce Marketing Cloud] ソース統合ではサポートされていません。

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL  カタログ ] には、Experience Platformでサポートされている様々なソースが表示されます。

カテゴリのリストから適切なカテゴリを選択できます。 検索バーを使用して、特定のソースをフィルタリングすることもできます。

[!UICONTROL  マーケティング自動化 ] カテゴリで、「**[!UICONTROL SalesforceMarketing Cloud]**」を選択し、**[!UICONTROL 設定]** を選択します。

![ ソースカタログとSalesforceMarketing Cloudソースが選択されています。](../../../../images/tutorials/create/salesforce-marketing-cloud/catalog.png)

**[!UICONTROL SalesforceMarketing Cloudに接続]** ページが表示されます。 このページでは、新しいアカウントを作成するか、既存のアカウントを使用できます。

### 新規アカウント

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、アカウントの名前、説明（オプション）、[!DNL Salesforce Marketing Cloud] アカウントに対応する認証資格情報を入力します。

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![Salesforce Marketing Cloudの新しいアカウントを認証できる新しいアカウントインターフェイス。](../../../../images/tutorials/create/salesforce-marketing-cloud/new.png)

### 既存のアカウント

既存のアカウントがある場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、表示されるリストから使用するアカウントを選択します。

![ 既存のSalesforce Marketing Cloudアカウントのリストから選択できる既存のアカウントインターフェイス。](../../../../images/tutorials/create/salesforce-marketing-cloud/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Salesforce Marketing Cloud] アカウントとExperience Platformの間の接続を確立しました。 次のチュートリアルに進み、[ マーケティング自動化データをExperience Platformに取り込むためのデータフローを作成 ](../../dataflow/marketing-automation.md) できるようになりました。
