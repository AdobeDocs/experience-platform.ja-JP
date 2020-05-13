---
title: Amazon Kinesisの宛先
seo-title: Amazon Kinesisの宛先
description: Amazon Kinesisストレージへのリアルタイムのアウトバウンド接続を作成して、Adobe Experience Platformからデータをストリーミングします。
seo-description: Amazon Kinesisストレージへのリアルタイムのアウトバウンド接続を作成して、Adobe Experience Platformからデータをストリーミングします。
translation-type: tm+mt
source-git-commit: 47e03d3f58bd31b1aec45cbf268e3285dd5921ea
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 6%

---


# （ベータ版）Amazon Kinesisの宛先


>[!IMPORTANT]
>
>Adobe Real-time CDPの [!DNL Amazon Kinesis] 宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

アマゾンウェブサービスによる [!DNL Kinesis Data Streams] サービスを使用すると、大量のデータレコードのストリームをリアルタイムで収集して処理できます。

Adobe Experience Platformからのデータをストリーミングするために、 [!DNL Amazon Kinesis] ストレージへのリアルタイムのアウトバウンド接続を作成できます。

* 詳しくは、 [!DNL Amazon Kinesis]Amazonのドキュメント [を参照してください](https://docs.aws.amazon.com/streams/latest/dev/introduction.html)。
* API呼び出しを [!DNL Amazon Kinesis] 使用して接続するには、 [ストリーミング宛先APIチュートリアルを参照してください](/help/rtcdp/destinations/streaming-destinations-api-tutorial.md)。
* Adobe Real-time CDPユーザーインターフェイス [!DNL Amazon Kinesis] を使用して接続するには、以下のセクションを参照してください。

![UIでのAmazon Kinesis](/help/rtcdp/destinations/assets/aws-kinesis-destination.png)


## 使用例 {#use-cases}

Amazon Kinesisなどのストリーミング送信先を使用すると、高価値のセグメント化イベントや関連するプロファイル属性を、お客様の希望するシステムに簡単にフィードできます。

例えば、見込み客が、「コンバージョンする傾向が高い」セグメントに該当するホワイトペーパーをダウンロードしたとします。 見込み客が属するセグメントをAmazon Kinesisの宛先にマッピングすると、Amazon Kinesisでこのイベントを受け取ることになります。 企業のITシステムで最も効果的に機能すると考えられるように、Do-It-Yoursenアプローチを採用し、イベントの上にビジネスロジックを記述することができます。

## 宛先の接続 {#connect-destination}

See [Cloud storage destinations workflow ](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)for instructions on how to connect to your cloud storage destinations, including those supported by [!DNL Amazon].

For [!DNL Amazon Kinesis] destinations, enter the following information in the create destination workflow:

### アカウントステップ内 {#account-step}

* **アマゾンウェブサービスのアクセスキーと秘密鍵**: で、アクセスキー — 秘密アクセスキーペアを生成 [!DNL Amazon Web Services]して、アドビのリアルタイムCDPアクセスを [!DNL Amazon Kinesis] アカウントに付与します。 詳しくは、 [アマゾンウェブサービスドキュメントを参照してください](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)。
* **region**: データをストリーミングする [!DNL Amazon Web Services] 領域を指定します。

![アカウント手順の入力フィールド](/help/rtcdp/destinations/assets/aws-kinesis-account-step.png)

### 認証手順の {#authentication-step}

* **名前**: 接続先の名前を指定 [!DNL Amazon Kinesis]
* **説明**: への接続の説明を入力し [!DNL Amazon Kinesis]ます。
* **stream**: アカウント内の既存のデータストリームの名前を指定し [!DNL Amazon Kinesis] ます。 Adobe Real-time CDPは、このストリームにデータをエクスポートします。

![認証手順の入力フィールド](/help/rtcdp/destinations/assets/aws-kinesis-authentication-step.png)

<!--

>[!IMPORTANT]
>
>Adobe Real-time CDP needs `write` permissions on the bucket object where the export files will be delivered.

-->

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。

## 書き出されたデータ {#exported-data}

書き出したエクスペリエンスプラットフォームのデータは、JSON形式 [!DNL Amazon Kinesis] で取得されます。 例えば、特定のセグメントから離脱したオーディエンスのハッシュ化された電子メールIDを含むイベントは、次のようになります。

```
{
   "segmentMembership":{
      "ups":{
         "7841ba61-23c1-4bb3-a495-00d695fe1e93":{
            "lastQualificationTime":"2020-03-03T21:24:39Z",
            "status":"exited"
         }
      }
   }
},
"identityMap":{
   "email_lc_sha256":[
      {
         "id":"655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
         "id":"66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
   ]
},
```



>[!MORELIKETHIS]
>
>* [Amazon Kinesisに接続し、API呼び出しを使用してデータをアクティブにする](/help/rtcdp/destinations/streaming-destinations-api-tutorial.md)
>* [Azureイベントハブの宛先](/help/rtcdp/destinations/azure-event-hubs-destination.md)
>* [宛先のタイプとカテゴリ](/help/rtcdp/destinations/destination-types.md)

