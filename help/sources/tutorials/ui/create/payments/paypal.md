---
keywords: Experience Platform；ホーム；人気のトピック；paypal;Paypal
solution: Experience Platform
title: UI での PayPal ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して PayPal ソース接続を作成する方法を説明します。
exl-id: bbd3f634-cb28-45d8-9b7b-ed3873101882
source-git-commit: 6b6bd67e70267e81c144c37549b0dcba20534eb6
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 46%

---

# UI での [!DNL PayPal] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL PayPal] Platform ユーザーインターフェイスを使用したソースコネクタ

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL PayPal] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/payments.md)

### 必要な認証情報の収集

次の項目にアクセスするには、 [!DNL PayPal] アカウントプラットフォームの場合は、次の値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `host` | の URL [!DNL PayPal] インスタンス。 |
| `clientID` | 次に関連付けられたクライアント ID: [!DNL PayPal] アプリケーション。 |
| `clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL PayPal] アプリケーション。 |

導入の詳細については、 [[!DNL PayPal] 文書](https://developer.paypal.com/docs/api/overview/#get-credentials)

## [!DNL PayPal] アカウントを接続

必要な資格情報を収集したら、以下の手順に従って [!DNL PayPal] アカウントを Platform にリンクできます。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL 支払い]** カテゴリ、選択 **[!UICONTROL PayPal]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL PayPal] コネクタ。

![カタログ](../../../../images/tutorials/create/paypal/catalog.png)

この **[!UICONTROL PayPal に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL PayPal] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/paypal/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL PayPal] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/paypal/existing.png)

## 次の手順

このチュートリアルでは、[!DNL PayPal] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データフローを設定して支払いデータを Platform に取り込む](../../dataflow/payments.md).
