---
keywords: Experience Platform；ホーム；人気のトピック；Amazon Redshift;Amazon Redshift;Redshift;Redshift
solution: Experience Platform
title: UI でのAmazon Redshift ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してAmazon Redshift ソース接続を作成する方法を説明します。
exl-id: 4faf3200-673b-4a20-8f94-d049e800444b
source-git-commit: 2fb972b0ec8d1f679c6ce104a439265b5cc4d535
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 53%

---

# の作成 [!DNL Amazon Redshift] UI のソース接続

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Amazon Redshift] （以下「」という。）[!DNL Redshift]&quot;) を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Redshift] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

 で [!DNL Redshift] アカウントにアクセスするには、次の値を指定する必要があります。[!DNL Platform]

| **認証情報** | **説明** |
| -------------- | --------------- |
| `server` | サーバーが [!DNL Redshift] アカウント |
| `username` | ユーザー名 [!DNL Redshift] アカウント |
| `password` | ユーザーに関連付けられたパスワード [!DNL Redshift] アカウント |
| `database` | この [!DNL Redshift] アクセスするデータベース。 |

の導入について詳しくは、 [この [!DNL Redshift] 文書](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html).

## [!DNL Redshift] アカウントを接続

>[!NOTE]
>
>のデフォルトのエンコーディング規格 [!DNL Redshift] は Unicode です。 これは変更できません。

必要な認証情報が揃ったら、次の手順に従って、[!DNL Redshift] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Amazon Redshift]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL Redshift] コネクタ。

![](../../../../images/tutorials/create/redshift/catalog.png)

この **[!UICONTROL Amazon Redshift に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL Redshift] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/redshift/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Redshift] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![](../../../../images/tutorials/create/redshift/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Redshift] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
