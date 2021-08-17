---
keywords: Amazon Kinesis;kinesis宛先；kinesis
title: Amazon Kinesis接続
description: Amazon Kinesisストレージへのリアルタイムアウトバウンド接続を作成し、Adobe Experience Platformからデータをストリーミングします。
exl-id: b40117ef-6ad0-48a9-bbcb-97c6f6d1dce3
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 3%

---

# （ベータ版） [!DNL Amazon Kinesis]接続

## 概要 {#overview}

>[!IMPORTANT]
>
>Platformの[!DNL Amazon Kinesis]宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

[!DNL Amazon Web Services]による[!DNL Kinesis Data Streams]サービスを使用すると、大量のデータレコードをリアルタイムで収集し、処理できます。

[!DNL Amazon Kinesis]ストレージへのリアルタイム送信接続を作成して、Adobe Experience Platformからデータをストリーミングできます。

* [!DNL Amazon Kinesis]について詳しくは、[Amazonのドキュメント](https://docs.aws.amazon.com/streams/latest/dev/introduction.html)を参照してください。
* プログラムで[!DNL Amazon Kinesis]に接続するには、『[ストリーミング宛先APIのチュートリアル](../../api/streaming-destinations.md)』を参照してください。
* Platformユーザーインターフェイスを使用して[!DNL Amazon Kinesis]に接続するには、以下の節を参照してください。

![Amazon KinesisとUI](../../assets/catalog/cloud-storage/amazon-kinesis/catalog.png)

## ユースケース {#use-cases}

[!DNL Amazon Kinesis]などのストリーミング宛先を使用すると、価値の高いセグメント化イベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、見込み客がホワイトペーパーをダウンロードし、それを「コンバージョン傾向が高い」セグメントに認定したとします。 見込み客が属するセグメントを[!DNL Amazon Kinesis]の宛先にマッピングすると、このイベントは[!DNL Amazon Kinesis]で受け取ります。 企業のITシステムと最適に連携すると考えられるように、Do-It-Yoursellアプローチを採用し、イベントの上にビジネスロジックを記述することができます。

## 書き出しタイプ {#export-type}

**プロファイルベースの**  — セグメントのすべてのメンバーを、目的のスキーマフィールド(例：電子メールアドレス、電話番号、姓)。オーディエンスアクティベーションワークフローの属性を選択画面 [から選択します](../../ui/activate-streaming-profile-destinations.md#select-attributes)。

## 必要な[!DNL Amazon Kinesis]権限 {#required-kinesis-permission}

[!DNL Amazon Kinesis]ストリームにデータを正常に接続してエクスポートするには、次のアクションに対するExperience Platformの権限が必要です。

* `kinesis:ListStreams`
* `kinesis:PutRecord`
* `kinesis:PutRecords`

これらの権限は、[!DNL Kinesis]コンソールを通じて設定され、PlatformユーザーインターフェイスでKinesisの宛先を設定すると、Platformによって確認されます。

次の例は、データを[!DNL Kinesis]宛先に正常にエクスポートするために必要な最小限のアクセス権を示しています。

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
| `kinesis:ListStreams` | Amazon Kinesisデータストリームをリストするアクション。 |
| `kinesis:PutRecord` | 単一のデータレコードをKinesisデータストリームに書き込むアクション。 |
| `kinesis:PutRecords` | 1回の呼び出しで複数のデータレコードをKinesisデータストリームに書き込むアクション。 |

[!DNL Kinesis]データストリームのアクセス制御の詳細については、以下の[[!DNL Kinesis] ドキュメント](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html)を参照してください。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!DNL Amazon Web Services]アクセスキーと秘密鍵**:で、 [!DNL Amazon Web Services]アカウントへの `access key - secret access key` アクセスをPlatformに付与するペアを生成 [!DNL Amazon Kinesis] します。詳しくは、[Amazon Webサービスのドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)を参照してください。
* **地域**:データをスト [!DNL Amazon Web Services] リーミングする地域を指定します。
* **名前**:接続先の名前を指定します。  [!DNL Amazon Kinesis]
* **説明**:への接続の説明を入力しま [!DNL Amazon Kinesis]す。
* **ストリーム**:アカウント内の既存のデータストリームの名前を指定 [!DNL Amazon Kinesis] します。プラットフォームは、このストリームにデータを書き出します。

<!--

>[!IMPORTANT]
>
>Platform needs `write` permissions on the bucket object where the export files will be delivered.

-->

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ストリーミングプロファイルの書き出し先へのオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md)を参照してください。

## エクスポートされたデータ {#exported-data}

書き出された[!DNL Experience Platform]データは、JSON形式で[!DNL Amazon Kinesis]に格納されます。 例えば、以下のイベントは、特定のセグメントに適合し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性を含みます。 この見込み客のIDはECIDと電子メールです。

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
>* [Amazon Kinesisに接続し、フローサービスAPIを使用してデータをアクティブ化する](../../api/streaming-destinations.md)
* [Azure Event Hubsの宛先](./azure-event-hubs.md)
* [宛先のタイプとカテゴリ](../../destination-types.md)

