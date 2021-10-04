---
keywords: Experience Platform；ホーム；人気のあるトピック；Salesforce;Salesforce
solution: Experience Platform
title: UI での Salesforce ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Salesforce ソース接続を作成する方法を説明します。
exl-id: b67fa4c4-d8ff-4d2d-aa76-5d9d32aa22d6
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 13%

---

# UI での [!DNL Salesforce] ソース接続の作成

Adobe Experience Platformのソースコネクタは、外部ソースの CRM データをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Salesforce] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Salesforce] アカウントがある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/crm.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

| 資格情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | [!DNL Salesforce] ソースインスタンスの URL。 |
| `username` | [!DNL Salesforce] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Salesforce] ユーザーアカウントのパスワード。 |
| `securityToken` | [!DNL Salesforce] ユーザーアカウントのセキュリティトークン。 |

開始方法の詳細は、[ この Salesforce ドキュメント ](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm) を参照してください。

## [!DNL Salesforce] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Salesforce] アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL Databases]**」カテゴリで、「**[!UICONTROL Salesforce]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい Salesforce コネクタを作成します。

![カタログ](../../../../images/tutorials/create/salesforce/catalog.png)

「**[!UICONTROL Salesforce に接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、[!DNL Salesforce] 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/salesforce/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Salesforce] アカウントを選択し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存](../../../../images/tutorials/create/salesforce/existing.png)

## 次の手順

このチュートリアルに従って、[!DNL Salesforce] アカウントへの接続を確立しました。 次のチュートリアルに進み、[ にデータを取り込むようにデータフローを設定します。 [!DNL Platform]](../../dataflow/crm.md)
