---
keywords: Amazon Kinesis; Kinesis 宛先; Kinesis
title: Amazon Kinesis 接続
description: Adobe エクスペリエンスプラットフォームからデータをストリーミングするために、Amazon Kinesis storage へのリアルタイムの送信接続を作成します。
exl-id: b40117ef-6ad0-48a9-bbcb-97c6f6d1dce3
source-git-commit: 2b1cde9fc913be4d3bea71e7d56e0e5fe265a6be
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 3%

---

# (ベータ版) [!DNL Amazon Kinesis] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>[!DNL Amazon Kinesis]プラットフォームの移行先は、現在ベータ版になっています。ドキュメントと機能は変更される場合があります。

このサービスにより、 [!DNL Kinesis Data Streams] [!DNL Amazon Web Services] 大量のデータレコードを収集し、リアルタイムに処理することができます。

[!DNL Amazon Kinesis]Adobe エクスペリエンスプラットフォームからデータをストリーミングするために、ストレージへのリアルタイムでの送信接続を作成することができます。

* について詳しくは [!DNL Amazon Kinesis] 、Amazon のマニュアルを参照してください [ ](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) 。
* プログラムに接続するには [!DNL Amazon Kinesis] 、 [ ストリーミング送信先 API のチュートリアルを参照してください ](../../api/streaming-destinations.md) 。
* プラットフォームのユーザーインターフェイスを使用してに接続するには、 [!DNL Amazon Kinesis] 以下の項を参照してください。

![UI の Amazon Kinesis](../../assets/catalog/cloud-storage/amazon-kinesis/catalog.png)

## ユースケース {#use-cases}

などのストリーミング出力を使用することにより、必要に応じて [!DNL Amazon Kinesis] 、高い値を持つセグメンテーションイベントおよび関連するプロファイル属性を、選択したシステムに簡単に送ることができます。

例えば、取引先によってホワイトペーパーがダウンロードされています。これにより、「propensity」セグメントに限定されます。 取引関係があるセグメントを宛先にマップすることによって [!DNL Amazon Kinesis] 、このイベントがに表示され [!DNL Amazon Kinesis] ます。 企業の IT システムに最適な機能として、開発者は独自のアプローチを採用し、イベントの上部にビジネスロジックを説明することができます。

## 書き出しタイプ {#export-type}

**プロファイルベース** -セグメントのすべてのメンバーを、出席者のアクティブ化ワークフローの「属性の選択」画面で選択したとおりに、目的のスキーマフィールド (例えば、電子メールアドレス、電話番号、姓) と共に書き出すことができ [ ](../../ui/activate-streaming-profile-destinations.md#select-attributes) ます。

## 必要な [!DNL Amazon Kinesis] 権限 {#required-kinesis-permission}

データをストリームに正しく接続して書き出すには [!DNL Amazon Kinesis] 、次の操作を実行するために必要なプラットフォームの権限が必要です。

* `kinesis:ListStreams`
* `kinesis:PutRecord`
* `kinesis:PutRecords`

これらの権限は、console によって表示され、 [!DNL Kinesis] プラットフォームのユーザーインターフェイスで Kinesis の出力先を設定すると、プラットフォームによってチェックされます。

次の例は、宛先にデータを正しく書き出すために必要な最低限のアクセス権を示して [!DNL Kinesis] います。

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kinesis:ListStreams",
                "kinesis:PutRecord",
                "kinesis:PutRecords"
            ],
            "Resource": [
                "arn:aws:kinesis:us-east-2:901341027596:stream/*"
            ]
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `kinesis:ListStreams` | Amazon Kinesis データストリームを一覧表示するアクションです。 |
| `kinesis:PutRecord` | 1つのデータレコードを Kinesis データストリームに書き込むアクション。 |
| `kinesis:PutRecords` | 複数のデータレコードを1回の呼び出しで Kinesis データストリームに書き込むアクション。 |

データストリームのアクセス制御について詳しくは [!DNL Kinesis] 、次のドキュメントを参照して [[!DNL Kinesis]  ](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html) ください。

## 目的の場所に接続します。 {#connect}

この送信先に接続するには、宛先の設定チュートリアルで説明されている手順に従って [ ](../../ui/connect-destination.md) ください。

### 接続パラメーター {#parameters}

このコピー先を設定する際に、 [ ](../../ui/connect-destination.md) 次の情報を入力する必要があります。

* **[!DNL Amazon Web Services]「In」キーと** 「秘密鍵」: 「In」により、プラットフォームへの [!DNL Amazon Web Services] `access key - secret access key` アクセスを許可するためのペアが生成さ [!DNL Amazon Kinesis] れます。 詳しくは、 [ Amazon Web サービスのマニュアルを参照 ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) してください。
* **region** : [!DNL Amazon Web Services] データをストリーミングする領域を指定します。
* **名前** : 接続の名前を指定します。 [!DNL Amazon Kinesis]
* **説明** : への接続に関する説明を入力 [!DNL Amazon Kinesis] します。
* **stream** : アカウントに既存のデータストリームの名前を指定し [!DNL Amazon Kinesis] ます。 プラットフォームによって、データがこのストリームに書き出されます。

<!--

>[!IMPORTANT]
>
>Platform needs `write` permissions on the bucket object where the export files will be delivered.

-->

## セグメントをこの宛先にアクティブにします。 {#activate}

[ ](../../ui/activate-streaming-profile-destinations.md) この宛先までの視聴ユーザーセグメントをアクティブにする方法については、「プロファイルの書き出し先のストリーミング送信先のストリーミングについて」を参照してください。

## 書き出したデータ {#exported-data}

書き出した [!DNL Experience Platform] データは [!DNL Amazon Kinesis] JSON 形式になります。 例えば、次のイベントには、特定のセグメントを対象としていて、別のセグメントを終了した対象ユーザーの電子メールアドレスプロファイル属性が含まれています。 このようなお客様の id は、d と email になります。

```json
{
  "person": {
    "email": "yourstruly@adobe.com"
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
>* [Amazon Kinesis に接続し、フローサービス API を使用してデータを有効にします。](../../api/streaming-destinations.md)
>* [Azure イベントハブの移行先](./azure-event-hubs.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)

