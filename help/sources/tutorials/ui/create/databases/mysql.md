---
keywords: Experience Platform；ホーム；人気のトピック；mysql;MySQL
solution: Experience Platform
title: UI での MySQL Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して MySQL ソース接続を作成する方法を説明します。
exl-id: 75e74bde-6199-4970-93d2-f95ec3a59aa5
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 37%

---

# UI での [!DNL MySQL] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、Adobe Experience Platform UI を使用して [!DNL MySQL] ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL MySQL] 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Experience Platform] で [!DNL MySQL] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | アカウントに関連付けられた [!DNL MySQL] の接続文字列。 [!DNL MySQL] の接続文字列パターンは `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` です。 接続文字列とその取得方法について詳しくは、[[!DNL MySQL]  ドキュメント ](https://dev.mysql.com/doc/connector-net/en/connector-net-connections-string.html) を参照してください。 |

## [!DNL MySQL] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL MySQL] アカウントを [!DNL Experience Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

**[!UICONTROL Databases]** カテゴリで、「**[!UICONTROL MySQL]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL MySQL] コネクタを作成します。

![](../../../../images/tutorials/create/my-sql/catalog.png)

**[!UICONTROL MySQL に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL MySQL] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![](../../../../images/tutorials/create/my-sql/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL MySQL] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![](../../../../images/tutorials/create/my-sql/existing.png)

## 次の手順

このチュートリアルでは、MySQL アカウントとの接続を確立しました。 次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/databases.md) を行いましょう。
