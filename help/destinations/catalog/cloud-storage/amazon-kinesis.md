---
keywords: Amazon Kinesis;kinesis destination;kinesis
title: AmazonKinesis駅
seo-title: AmazonKinesis駅
description: Adobe Experience Platformからのデータをストリーミングするために、AmazonKinesisストレージへのリアルタイムの発信接続を作成します。
seo-description: Adobe Experience Platformからのデータをストリーミングするために、AmazonKinesisストレージへのリアルタイムの発信接続を作成します。
translation-type: tm+mt
source-git-commit: 7484e64d0d359f40ef242dfc9d2d1704018a8ed6
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 7%

---


# （ベータ） [!DNL Amazon Kinesis] 宛先


>[!IMPORTANT]
>
>リアルタイムCDPの [!DNL Amazon Kinesis] 宛先は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

による [!DNL Kinesis Data Streams][!DNL Amazon Web Services] サービスでは、大量のデータレコードのストリームをリアルタイムで収集して処理できます。

Adobe Experience Platformからのデータをストリーミングするために、 [!DNL Amazon Kinesis] ストレージへのリアルタイムの送信接続を作成できます。

* 詳しくは、 [!DNL Amazon Kinesis]Amazonのドキュメント [を参照してください](https://docs.aws.amazon.com/streams/latest/dev/introduction.html)。
* API呼び出しを [!DNL Amazon Kinesis] 使用して接続するには、 [ストリーミング宛先APIチュートリアルを参照してください](../../api/streaming-destinations.md)。
* Real-time CDPユーザー・インターフェイスを [!DNL Amazon Kinesis] 使用して接続するには、以下のセクションを参照してください。

![AmazonKinesisUI](../../assets/catalog/cloud-storage/amazon-kinesis/catalog.png)


## 使用例 {#use-cases}

などのストリーミング送信先を使用すると [!DNL Amazon Kinesis]、高価値のセグメントイベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、見込み客が、「コンバージョンする傾向が高い」セグメントに該当するホワイトペーパーをダウンロードしたとします。 見込み客が属するセグメントを [!DNL Amazon Kinesis] 宛先にマッピングすると、このイベントがに表示され [!DNL Amazon Kinesis]ます。 企業のITシステムで最も効果的に機能すると考えられるように、Do-It-Yoursenアプローチを採用し、イベントの上にビジネスロジックを記述することができます。

## 書き出しタイプ {#export-type}

**プロファイルベース** — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## 宛先の接続 {#connect-destination}

See [Cloud storage destinations workflow ](./workflow.md)for instructions on how to connect to your cloud storage destinations, including those supported by [!DNL Amazon].

For [!DNL Amazon Kinesis] destinations, enter the following information in the create destination workflow:

### 認証手順の {#authentication-step}

* **[!DNL Amazon Web Services]アクセスキーと秘密鍵**:では、 [!DNL Amazon Web Services]ペアを作成して、お客様のア `access key - secret access key`[!DNL Amazon Kinesis] カウントにリアルタイムCDPアクセスを許可します。 詳しくは、 [Amazonウェブサービスのドキュメントを参照してください](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)。
* **region**:データをストリーミングする [!DNL Amazon Web Services] 領域を指定します。

![アカウント手順の入力フィールド](../../assets/catalog/cloud-storage/amazon-kinesis/account.png)

### 設定手順で {#setup-step}

* **名前**:接続先の名前を指定 [!DNL Amazon Kinesis]
* **説明**:への接続の説明を入力し [!DNL Amazon Kinesis]ます。
* **stream**:アカウント内の既存のデータストリームの名前を指定し [!DNL Amazon Kinesis] ます。 リアルタイムCDPは、このストリームにデータをエクスポートします。

![認証手順の入力フィールド](../../assets/catalog/cloud-storage/amazon-kinesis/setup.png)

<!--

>[!IMPORTANT]
>
>Real-time CDP needs `write` permissions on the bucket object where the export files will be delivered.

-->

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## 書き出されたデータ {#exported-data}

書き出された [!DNL Experience Platform] データはJSON形式 [!DNL Amazon Kinesis] で取得されます。 例えば、次のイベントには、特定のセグメントに該当し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性が含まれています。 この見込み客のIDは、ECIDと電子メールです。

```json
{
  "person": {
    "email": "yourstruly@adobe.con"
  },
  "segmentMembership": {
    "ups": {
      "7841ba61-23c1-4bb3-a495-00d3g5fe1e93": {
        "lastQualificationTime": "2020-05-25T21:24:39Z",
        "status": "exited"
      },
      "59bd2fkd-3c48-4b18-bf56-4f5c5e6967ae": {
        "lastQualificationTime": "2020-05-25T23:37:33Z",
        "status": "existing"
      }
    }
  },
  "identityMap": {
    "ecid": [
      {
        "id": "14575006536349286404619648085736425115"
      },
      {
        "id": "66478888669296734530114754794777368480"
      }
    ],
    "email_lc_sha256": [
      {
        "id": "655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
        "id": "66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
    ]
  }
}
```



>[!MORELIKETHIS]
>
>* [AmazonKinesisに接続し、API呼び出しを使用してデータをアクティブにする](../../api/streaming-destinations.md)
>* [Azureイベントハブの宛先](./azure-event-hubs.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)

