---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UI での ServiceNow ソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 41fe3e5b2a830c3182b46b3e0873b1672a1f1b03
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 16%

---


# Create a [!DNL ServiceNow] source connector in the UI

>[!NOTE]
>コネクタ [!DNL ServiceNow] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL ServiceNow] ザインタフェースを使用して [!DNL Platform] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に接続している場合は、このドキュメントの残りの部分をスキップして、データフローの [!DNL ServiceNow][設定に関するチュートリアルに進むことができます](../../dataflow/customer-success.md)

### 必要な資格情報の収集

で [!DNL ServiceNow] アカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `endpoint` | サー [!DNL ServiceNow] バーのエンドポイント。 |
| `username` | 認証のためにサー [!DNL ServiceNow] バーに接続するために使用するユーザー名です。 |
| `password` | 認証用に [!DNL ServiceNow] サーバーに接続するためのパスワードです。 |

使い始める方法の詳細については、 [このServiceNowドキュメントを参照してください](https://developer.servicenow.com/app.do#!/rest_api_doc?v=newyork&amp;id=r_TableAPI-GET)。

## アカウントに接続 [!DNL ServiceNow] する

必要な資格情報を収集したら、次の手順に従って、接続する新しい [!DNL ServiceNow] アカウントを作成でき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 カ *[!UICONTROL タログ]* 画面には様々なソースが表示され、このソースからアカウントを作成できます。各ソースには既存のアカウントの数と、それらに関連付けられたデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL Customer Success]* ( **[!UICONTROL 顧客成功]** )」カテゴリの下で「ServiceNow（サービス提供）」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しいアカウントを作成するには、「 **[!UICONTROL 接続元]**」を選択します。

![](../../../../images/tutorials/create/servicenow/catalog.png)

「ServiceNow *[!UICONTROL に接続]* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明および [!DNL ServiceNow] 資格情報を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/servicenow/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL ServiceNow] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../../../../images/tutorials/create/servicenow/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL ServiceNow] カウントへの接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/customer-success.md)。
