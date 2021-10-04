---
keywords: Experience Platform；ホーム；人気のあるトピック；Salesforce Service Cloud;Salesforce サービスクラウド
solution: Experience Platform
title: UI での Salesforce サービスクラウドソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Salesforce Service Cloud ソース接続を作成する方法を説明します。
exl-id: 38480a29-7852-46c6-bcea-5dc6bffdbd15
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 12%

---

# UI での [!DNL Salesforce Service Cloud] ソース接続の作成

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Salesforce Service Cloud]（以下「SSC」と呼ばれます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な SSC 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/customer-success.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL Platform] の SSC アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `username` | ユーザーアカウントのユーザー名。 |
| `password` | ユーザーアカウントのパスワード。 |
| `securityToken` | ユーザーアカウントのセキュリティトークン。 |

使い始める方法については、[ この  [!DNL Salesforce Service Cloud]  ドキュメント ](https://developer.salesforce.com/docs/atlas.en-us.api_iot.meta/api_iot/qs_auth_access_token.htm) を参照してください。

## [!DNL Salesforce Service Cloud] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って SSC アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL 顧客の成功]**」カテゴリで、「**[!UICONTROL Salesforce Service Cloud]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL Add data]**」を選択して新しい SSC コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ssc/catalog.png)

「**[!UICONTROL Salesforce Service Cloud に接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、SSC 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/ssc/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する SSC アカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![既存](../../../../images/tutorials/create/ssc/existing.png)

## 次の手順

このチュートリアルに従って、SSC アカウントへの接続を確立しました。 次のチュートリアルに進み、顧客成功データを  [!DNL Platform]](../../dataflow/customer-success.md) に取り込むように [ データフローを設定します。
