---
keywords: Experience Platform；ホーム；人気のトピック；paypal;Paypal
solution: Experience Platform
title: UI での PayPal Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して PayPal ソース接続を作成する方法を説明します。
exl-id: bbd3f634-cb28-45d8-9b7b-ed3873101882
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 38%

---

# UI での [!DNL PayPal] ソース接続の作成

>[!WARNING]
>
>[!DNL PayPal] ソースは 2025 年 6 月末に非推奨（廃止予定）になります。

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用して [!DNL PayPal] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL PayPal] 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/payments.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL PayPal] アカウントのExperience Platformにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL PayPal] インスタンスの URL。 |
| `clientID` | [!DNL PayPal] アプリケーションに関連付けられたクライアント ID。 |
| `clientSecret` | [!DNL PayPal] アプリケーションに関連付けられたクライアント秘密鍵。 |

基本について詳しくは、この [[!DNL PayPal]  ドキュメント ](https://developer.paypal.com/docs/api/overview/#get-credentials) を参照してください。

## [!DNL PayPal] アカウントを接続

必要な資格情報を収集したら、次の手順に従って [!DNL PayPal] アカウントをExperience Platformにリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL 支払い]** カテゴリで、**[!UICONTROL PayPal]** を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL PayPal] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/paypal/catalog.png)

**[!UICONTROL PayPal に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL PayPal] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 接続 ](../../../../images/tutorials/create/paypal/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL PayPal] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/paypal/existing.png)

## 次の手順

このチュートリアルでは、[!DNL PayPal] アカウントとの接続を確立しました。次のチュートリアルに進み、[ 支払いデータをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/payments.md) を行いましょう。
