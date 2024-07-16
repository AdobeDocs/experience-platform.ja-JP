---
title: UI でのAmazon Kinesis Source接続の作成
description: Adobe Experience Platform UI を使用してAmazon Kinesis ソース接続を作成する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 4152e48b-bec7-4b05-a172-eea71c9d9880
source-git-commit: 9a8139c26b5bb5ff937a51986967b57db58aab6c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 41%

---

# UI での [!DNL Amazon Kinesis] ソース接続の作成

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Amazon Kinesis] ソースを利用できます。

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Amazon Kinesis] （以下「[!DNL "Kinesis"]」と呼びます）ソースコネクタを認証する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Kinesis] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/streaming/cloud-storage-streaming.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Kinesis] ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `accessKeyId` | [!DNL Kinesis] アカウントのアクセスキー ID。 |
| `Secret access key` | [!DNL Kinesis] アカウントの秘密アクセスキー。 |
| `region` | AWS サーバーの地域。 |

これらの値について詳しくは、[ この  [!DNL Kinesis]  ドキュメント ](https://docs.aws.amazon.com/streams/latest/dev/getting-started.html) を参照してください。

## [!DNL Kinesis] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL Kinesis] アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL クラウドストレージ]** カテゴリで、「**[!UICONTROL Amazon Kinesis]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Kinesis] コネクタを作成します。

![](../../../../images/tutorials/create/kinesis/catalog.png)

**[!UICONTROL Amazon Kinesisに接続]** ダイアログが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、[!DNL Kinesis] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![](../../../../images/tutorials/create/kinesis/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Kinesis] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![](../../../../images/tutorials/create/kinesis/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Kinesis] アカウントに接続し [!DNL Platform] す。 次のチュートリアルに進み、[ クラウドストレージからデータをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/streaming/cloud-storage-streaming.md) を行いましょう。
