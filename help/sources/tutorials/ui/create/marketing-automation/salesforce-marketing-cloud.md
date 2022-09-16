---
keywords: Experience Platform；ホーム；人気の高いトピック；salesforce marketing cloud;Salesforce Marketing Cloud
solution: Experience Platform
title: UI での SalesforceMarketing Cloudソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して SalesforceMarketing Cloudソース接続を作成する方法を説明します。
exl-id: 1d9bde60-31e0-489c-9c1c-b6471e0ea554
source-git-commit: 531d5619e0643b6195abaa53d1708e0368d45871
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 44%

---

# UI での [!DNL Salesforce Marketing Cloud] ソース接続の作成

>[!NOTE]
>
> この [!DNL Salesforce Marketing Cloud] ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Salesforce Marketing Cloud] Platform ユーザーインターフェイスを使用したソースコネクタ

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL Salesforce Marketing Cloud] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/marketing-automation.md).

### 必要な認証情報の収集

Platform で [!DNL Salesforce Marketing Cloud] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アプリケーションのホストサーバー。 多くの場合、これはサブドメインです。 **注意：** を `host` の値を指定する場合、URL 全体ではなくサブドメインのみを指定する必要があります。 例えば、ホスト URL が `https://abcd-ab12c3d4e5fg6hijk7lmnop8qrst.auth.marketingcloudapis.com/`を指定した場合は、 `abcd-ab12c3d4e5fg6hijk7lmnop8qrst` をホスト値として使用します。 |
| `clientId` | 次に関連付けられたクライアント ID: [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Salesforce Marketing Cloud] アプリケーション。 |

導入の詳細については、 [[!DNL Salesforce Marketing Cloud] 文書](https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/authentication.htm).

## [!DNL Salesforce Marketing Cloud] アカウントを接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Salesforce Marketing Cloud] アカウントを Platform にリンクできます。

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。検索バーを使用して、表示されるコネクタを絞り込むこともできます。

以下 [!UICONTROL マーケティングの自動化] カテゴリ、選択 **[!UICONTROL SalesforceMarketing Cloud]** 次に、 **[!UICONTROL 設定]**.

![カタログ](../../../../images/tutorials/create/salesforce-marketing-cloud/catalog.png)

この **[!UICONTROL SalesforceMarketing Cloudに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL Salesforce Marketing Cloud] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![新規](../../../../images/tutorials/create/salesforce-marketing-cloud/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Salesforce Marketing Cloud] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/salesforce-marketing-cloud/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Salesforce Marketing Cloud] アカウントとの接続を確立しました。次のチュートリアルに進み、 [マーケティング自動化システムデータを Platform に取り込むためのデータフローの設定](../../dataflow/marketing-automation.md).
