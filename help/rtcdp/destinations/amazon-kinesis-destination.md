---
title: Amazon Kinesisの宛先
seo-title: Amazon Kinesisの宛先
description: Amazon Kinesisストレージへのリアルタイムのアウトバウンド接続を作成して、Adobe Experience Platformからデータをストリーミングします。
seo-description: Amazon Kinesisストレージへのリアルタイムのアウトバウンド接続を作成して、Adobe Experience Platformからデータをストリーミングします。
translation-type: tm+mt
source-git-commit: b96286f6a06f0583b45343a513ee64f0025d79a7
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 7%

---


# （ベータ） [!DNL Amazon Kinesis] 宛先


>[!IMPORTANT]
>
>Adobe Real-time CDPの [!DNL Amazon Kinesis] 宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

によるこの [!DNL Kinesis Data Streams][!DNL Amazon Web Services] サービスでは、大量のデータレコードのストリームをリアルタイムで収集して処理できます。

Adobe Experience Platformからデータをストリーミングするために、 [!DNL Amazon Kinesis] ストレージへのリアルタイムの送信接続を作成できます。

* 詳しくは、 [!DNL Amazon Kinesis]Amazonのドキュメント [を参照してください](https://docs.aws.amazon.com/streams/latest/dev/introduction.html)。
* API呼び出しを [!DNL Amazon Kinesis] 使用して接続するには、 [ストリーミング宛先APIチュートリアルを参照してください](/help/rtcdp/destinations/streaming-destinations-api-tutorial.md)。
* Adobe Real-time CDPユーザーインターフェイス [!DNL Amazon Kinesis] を使用して接続するには、以下のセクションを参照してください。

![UIでのAmazon Kinesis](/help/rtcdp/destinations/assets/aws-kinesis-destination.png)


## 使用例 {#use-cases}

などのストリーミング送信先を使用すると [!DNL Amazon Kinesis]、高価値のセグメントイベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、見込み客が、「コンバージョンする傾向が高い」セグメントに該当するホワイトペーパーをダウンロードしたとします。 見込み客が属するセグメントを [!DNL Amazon Kinesis] 宛先にマッピングすると、このイベントがに表示され [!DNL Amazon Kinesis]ます。 企業のITシステムで最も効果的に機能すると考えられるように、Do-It-Yoursenアプローチを採用し、イベントの上にビジネスロジックを記述することができます。

## 宛先の接続 {#connect-destination}

See [Cloud storage destinations workflow ](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)for instructions on how to connect to your cloud storage destinations, including those supported by [!DNL Amazon].

For [!DNL Amazon Kinesis] destinations, enter the following information in the create destination workflow:

### 認証手順の {#authentication-step}

* **[!DNL Amazon Web Services]アクセスキーと秘密鍵&#x200B;**: で、アクセスキー — 秘密アクセスキーペアを生成[!DNL Amazon Web Services]して、アドビのリアルタイムCDPアクセスを[!DNL Amazon Kinesis]アカウントに付与します。 詳しくは、[アマゾンウェブサービスドキュメントを参照してください](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)。
* **region**: データをストリーミングする [!DNL Amazon Web Services] 領域を指定します。

![アカウント手順の入力フィールド](/help/rtcdp/destinations/assets/aws-kinesis-account-step.png)

### 設定手順で {#setup-step}

* **名前**: 接続先の名前を指定 [!DNL Amazon Kinesis]
* **説明**: への接続の説明を入力し [!DNL Amazon Kinesis]ます。
* **stream**: アカウント内の既存のデータストリームの名前を指定し [!DNL Amazon Kinesis] ます。 Adobe Real-time CDPは、このストリームにデータをエクスポートします。

![認証手順の入力フィールド](/help/rtcdp/destinations/assets/aws-kinesis-setup-step.png)

<!--

>[!IMPORTANT]
>
>Adobe Real-time CDP needs `write` permissions on the bucket object where the export files will be delivered.

-->

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。

## 書き出されたデータ {#exported-data}

書き出された [!DNL Experience Platform] データはJSON形式 [!DNL Amazon Kinesis] で取得されます。 例えば、次のイベントには、特定のセグメントに該当し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性が含まれています。 この見込み客のIDは、ECIDと電子メールです。

```
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
>* [Amazon Kinesisに接続し、API呼び出しを使用してデータをアクティブにする](/help/rtcdp/destinations/streaming-destinations-api-tutorial.md)
>* [Azureイベントハブの宛先](/help/rtcdp/destinations/azure-event-hubs-destination.md)
>* [宛先のタイプとカテゴリ](/help/rtcdp/destinations/destination-types.md)

