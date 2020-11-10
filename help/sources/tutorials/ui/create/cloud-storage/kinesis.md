---
keywords: Experience Platform;home;popular topics;Amazon Kinesis;amazon kinesis;Kinesis;kinesis
solution: Experience Platform
title: UIでAmazonKinesisソースコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、プラットフォームユーザーインターフェイスを使用して、AmazonKinesis(以下「Kinesis」と呼ばれる)ソースコネクタを認証する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 9%

---


# Create an [!DNL Amazon Kinesis] source connector in the UI

>[!NOTE]
>
>コネクタ [!DNL Amazon Kinesis] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Amazon Kinesis] ユーザーインターフェイスを使用して [!DNL "Kinesis"](以下「 [!DNL Platform] 」と呼びます)ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な [!DNL Kinesis] 接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/streaming/cloud-storage-streaming.md)。

### 必要な資格情報の収集

ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があり [!DNL Kinesis] ます。

| Credential | 説明 |
| ---------- | ----------- |
| `accessKeyId` | アカウントのアクセスキーID [!DNL Kinesis] 。 |
| `Secret access key` | アカウントの秘密アクセスキー [!DNL Kinesis] 。 |
| `region` | AWSサーバーの地域です。 |

これらの値の詳細については、 [ [!DNL Kinesis] このドキュメントを参照してください](https://docs.aws.amazon.com/streams/latest/dev/getting-started.html)。

## アカウントに接続 [!DNL Kinesis] する

必要な資格情報を収集したら、次の手順に従って [!DNL Kinesis] アカウントをにリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には、アカウントを作成する際に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL クラウドストレージ]** カテゴリの下で、「 **[!UICONTROL AmazonKinesis]**」を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL 追加]** データ [!DNL Kinesis] を選択して新しいコネクタを作成します。

![](../../../../images/tutorials/create/kinesis/catalog.png)

[ **[!UICONTROL AmazonKinesisに]** 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および [!DNL Kinesis] 資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/kinesis/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Kinesis] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../../../../images/tutorials/create/kinesis/existing.png)

## 次の手順

このチュートリアルに従って、に [!DNL Kinesis] アカウントに接続し [!DNL Platform]ました。 次のチュートリアルに進み、クラウドストレージのデータをに取り込むようにデータフローを [設定できます [!DNL Platform]](../../dataflow/streaming/cloud-storage-streaming.md)。