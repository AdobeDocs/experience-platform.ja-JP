---
keywords: Experience Platform；ホーム；人気のあるトピック；ペイパル；Paypal
solution: Experience Platform
title: UI での PayPal ソースコネクタの作成
topic: overview
type: Tutorial
description: このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してPayPalソースコネクタを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 11%

---


# UIに[!DNL PayPal]ソースコネクタを作成する

>[!NOTE]
>
> [!DNL PayPal]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL PayPal]ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL PayPal]接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/payments.md)のチュートリアルに進むことができます

### 必要な資格情報の収集

[!DNL PayPal]アカウント[!DNL Platform]にアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | [!DNL PayPal]インスタンスのURL。 |
| `clientID` | [!DNL PayPal]アプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | [!DNL PayPal]アプリケーションに関連付けられているクライアントシークレット。 |

開始方法の詳細については、[[!DNL PayPal] ドキュメント](https://developer.paypal.com/docs/api/overview/#get-credentials)を参照してください

## [!DNL PayPal]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL PayPal]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「**[!UICONTROL 支払い]**」カテゴリで、「**[!UICONTROL PayPal]**」を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL PayPal]コネクタを作成します。

![カタログ](../../../../images/tutorials/create/paypal/catalog.png)

**[!UICONTROL PayPal]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL PayPal]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/paypal/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL PayPal]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/paypal/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL PayPal]アカウントへの接続が確立されます。 次のチュートリアルに進み、[支払いデータを [!DNL Platform]](../../dataflow/payments.md)に送るようにデータフローを設定できます。