---
title: UI でのAmazon Redshift Source接続の作成
description: Adobe Experience Platform UI を使用してAmazon Redshift ソース接続を作成する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 4faf3200-673b-4a20-8f94-d049e800444b
source-git-commit: a7c2c5e4add5c80e0622d5aeb766cec950d79dbb
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 41%

---

# ソースワークスペースを使用して [!DNL Amazon Redshift] アカウントに接続します

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Amazon Redshift] ソースを利用できます。

このチュートリアルでは、ユーザーインターフェイスを使用して [!DNL Amazon Redshift] （以下「[!DNL Redshift]」）アカウントをAdobe Experience Platformに接続する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Redshift] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

Experience Platformで [!DNL Redshift] アカウントにアクセスするには、次の値を指定する必要があります。

| **認証情報** | **説明** |
| -------------- | --------------- |
| サーバー | [!DNL Redshift] アカウントに関連付けられたサーバー。 |
| ポート | クライアント接続をリッスンするために [!DNL Redshift] サーバーが使用する TCP ポート。 |
| ユーザー名 | [!DNL Redshift] アカウントに関連付けられたユーザー名。 |
| パスワード | [!DNL Redshift] アカウントに関連付けられたパスワード。 |
| データベース | アクセスする [!DNL Redshift] データベース。 |

基本について詳しくは、[ このドキュメント  [!DNL Redshift]  を参照してください ](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)。

必要な資格情報を収集したら、次の手順に従って [!DNL Redshift] アカウントをExperience Platformにリンクできます。

## [!DNL Redshift] アカウントを接続

>[!NOTE]
>
>[!DNL Redshift] のデフォルトのエンコーディング規格は Unicode です。 これは変更できません。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Databases]** カテゴリで、**[!UICONTROL Amazon Redshift]** を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Redshift] コネクタを作成します。

![](../../../../images/tutorials/create/redshift/catalog.png)

**[!UICONTROL Amazon Redshift に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Redshift] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![](../../../../images/tutorials/create/redshift/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Redshift] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![](../../../../images/tutorials/create/redshift/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Redshift] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/databases.md) を行いましょう。
