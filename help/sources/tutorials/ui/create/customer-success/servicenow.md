---
keywords: Experience Platform；ホーム；人気のあるトピック；ServiceNow;servicenow
solution: Experience Platform
title: UIでのServiceNowソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してServiceNowソース接続を作成する方法を説明します。
exl-id: 66c12f4d-8b0c-4bb2-910d-9e09fa364c94
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 9%

---

# UIに[!DNL ServiceNow]ソース接続を作成する

>[!NOTE]
>
>[!DNL ServiceNow]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL ServiceNow]ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL ServiceNow]接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/customer-success.md)のチュートリアルに進むことができます

### 必要な資格情報の収集

[!DNL Platform]の[!DNL ServiceNow]アカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `endpoint` | [!DNL ServiceNow]サーバーのエンドポイント。 |
| `username` | 認証用に[!DNL ServiceNow]サーバーに接続するために使用するユーザー名です。 |
| `password` | 認証用に[!DNL ServiceNow]サーバーに接続するためのパスワードです。 |

開始方法の詳細については、[この [!DNL ServiceNow] ドキュメント](https://developer.servicenow.com/app.do#!/rest_api_doc?v=newyork&amp;id=r_TableAPI-GET)を参照してください。

## [!DNL ServiceNow]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL ServiceNow]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL Customer Success]**&#x200B;カテゴリの下で、**[!UICONTROL ServiceNow]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、「**[!UICONTROL ソースを接続]**」を選択して、新しい[!DNL ServiceNow]コネクタを作成します。

![](../../../../images/tutorials/create/servicenow/catalog.png)

**[!UICONTROL ServiceNowに接続]**&#x200B;ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL ServiceNow]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/servicenow/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL ServiceNow]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![](../../../../images/tutorials/create/servicenow/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL ServiceNow]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/customer-success.md)に取り込むようにデータフローを設定できます。
