---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでAmazonKinesisソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 41fe3e5b2a830c3182b46b3e0873b1672a1f1b03
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 17%

---


# Create an [!DNL Amazon Kinesis] source connector in the UI

>[!NOTE]
>コネクタ [!DNL Amazon Kinesis] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Amazon Kinesis] ユーザーインターフェイスを使用して [!DNL "Kinesis"](以下「 [!DNL Platform] 」と呼びます)ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に [!DNL Kinesis] アカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/streaming/cloud-storage.md)。

### 必要な資格情報の収集

ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があり [!DNL Kinesis] ます。

| Credential | 説明 |
| ---------- | ----------- |
| `accessKeyId` | アカウントのアクセスキーID [!DNL Kinesis] 。 |
| `Secret access key` | アカウントの秘密アクセスキー [!DNL Kinesis] 。 |
| `region` | AWSサーバーの地域です。 |

これらの値の詳細については、 [このKinesisドキュメントを参照してください](https://docs.aws.amazon.com/streams/latest/dev/getting-started.html)。

## アカウントに接続 [!DNL Kinesis] する

必要な資格情報を収集したら、次の手順に従って [!DNL Kinesis] アカウントをにリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 *ソース* 」ワークスペースにアクセスします。 「 *カタログ* 」タブには、接続可能な様々なソースが表示され [!DNL Platform]ます。 各ソースには、関連付けられた既存のアカウントの数が表示されます。

[ *[!UICONTROL クラウドストレージ]***カテゴリで、[]** AmazonKinesis **** ]を選択し、次に新しい [!DNL Kinesis] コネクタを作成するデータを選択します。

![](../../../../images/tutorials/create/kinesis/catalog.png)

[ *[!UICONTROL AmazonKinesisに]* 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および [!DNL Kinesis] 資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/kinesis/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Kinesis] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../../../../images/tutorials/create/kinesis/existing.png)

## 次の手順

このチュートリアルに従って、に [!DNL Kinesis] アカウントに接続し [!DNL Platform]ました。 次のチュートリアルに進み、クラウドストレージのデータをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/streaming/cloud-storage.md)。