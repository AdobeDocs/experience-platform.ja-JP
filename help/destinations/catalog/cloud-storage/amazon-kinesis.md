---
keywords: Amazon Kinesis;kinesis destination;kinesis
title: Amazon Kinesis接続
description: Amazon Kinesisストレージへのリアルタイムアウトバウンド接続を作成し、Adobe Experience Platformからデータをストリーミングします。
exl-id: b40117ef-6ad0-48a9-bbcb-97c6f6d1dce3
source-git-commit: f7f3bc229ddad046dca5ea8d2889942fc9cb2cab
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 2%

---

# （ベータ版） [!DNL Amazon Kinesis] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>この [!DNL Amazon Kinesis] の宛先は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

この [!DNL Kinesis Data Streams] サービス [!DNL Amazon Web Services] を使用すると、大量のデータレコードをリアルタイムで収集して処理できます。

へのリアルタイムアウトバウンド接続を作成できます [!DNL Amazon Kinesis] Adobe Experience Platformからデータをストリーミングするためのストレージ。

* 詳しくは、 [!DNL Amazon Kinesis]を参照し、 [Amazonドキュメント](https://docs.aws.amazon.com/streams/latest/dev/introduction.html).
* 接続するには [!DNL Amazon Kinesis] プログラムを使用して、 [ストリーミング宛先 API のチュートリアル](../../api/streaming-destinations.md).
* 接続するには [!DNL Amazon Kinesis] Platform ユーザーインターフェイスを使用する場合は、以下の節を参照してください。

![UI でのAmazon Kinesis](../../assets/catalog/cloud-storage/amazon-kinesis/catalog.png)

## ユースケース {#use-cases}

次のようなストリーミング宛先を使用する [!DNL Amazon Kinesis]を使用すると、高価値のセグメントイベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、見込み客がホワイトペーパーをダウンロードし、それを「コンバージョン傾向が高い」セグメントに認定したとします。 見込み客が属するセグメントを [!DNL Amazon Kinesis] の宛先に指定した場合、このイベントは [!DNL Amazon Kinesis]. 企業の IT システムに最適に対応できると思われるように、このイベントの上に、自らのアプローチを使用し、ビジネスロジックを説明することができます。

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。 [オーディエンスのアクティベーションワークフロー](../../ui/activate-streaming-profile-destinations.md#select-attributes).

## 必須 [!DNL Amazon Kinesis] 権限 {#required-kinesis-permission}

データを正常に接続してに書き出すには、以下を実行します。 [!DNL Amazon Kinesis] ストリーム、Experience Platformには次のアクションに対する権限が必要です：

* `kinesis:ListStreams`
* `kinesis:PutRecord`
* `kinesis:PutRecords`

これらの権限は、 [!DNL Kinesis] コンソールとは、Platform ユーザーインターフェイスでKinesisの宛先を設定すると、Platform によって確認されます。

次の例は、データをに正常に書き出すために必要な最小限のアクセス権を示しています [!DNL Kinesis] 宛先。

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
| `kinesis:ListStreams` | Amazon Kinesisのデータストリームをリストするアクション。 |
| `kinesis:PutRecord` | 単一のデータレコードをKinesisデータストリームに書き込むアクション。 |
| `kinesis:PutRecords` | 1 回の呼び出しで複数のデータレコードをKinesisデータストリームに書き込むアクション。 |

のアクセス制御の詳細 [!DNL Kinesis] データストリーム、以下を読む [[!DNL Kinesis] 文書](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html).

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!DNL Amazon Web Services]アクセスキーと秘密鍵**:In [!DNL Amazon Web Services]、 `access key - secret access key` ペアを使用して、 [!DNL Amazon Kinesis] アカウント 詳しくは、 [Amazon Web Servicesドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html).
* **地域**:対象を指定 [!DNL Amazon Web Services] データのストリーミング先の地域。
* **名前**:接続先の名前を指定してください [!DNL Amazon Kinesis]
* **説明**:接続先の説明を入力してください [!DNL Amazon Kinesis].
* **ストリーム**:の既存のデータストリームの名前を指定します。 [!DNL Amazon Kinesis] アカウント Platform はこのストリームにデータを書き出します。

<!--

>[!IMPORTANT]
>
>Platform needs `write` permissions on the bucket object where the export files will be delivered.

-->

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md) を参照してください。

## プロファイルの書き出し動作 {#profile-export-behavior}

Experience Platformは、セグメントの選定または他の重要なイベントに従ってプロファイルに関連する更新が発生した場合にのみ、プロファイルの書き出し動作をAmazon Kinesisの宛先に最適化します。 プロファイルは、次の状況で宛先に書き出されます。

* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つのセグメントのセグメントメンバーシップの変更によってトリガーされました。 例えば、プロファイルは、宛先にマッピングされたいずれかのセグメントに適合しているか、宛先にマッピングされたいずれかのセグメントから退出しています。
* プロファイルの更新は、 [id マップ](/help/xdm/field-groups/profile/identitymap.md). 例えば、宛先にマッピングされたセグメントの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つの属性の属性の変更によってトリガーされました。 例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合、関連する更新がおこなわれたプロファイルのみが宛先にエクスポートされます。 例えば、宛先フローにマッピングされたセグメントのメンバーが 100 人で、5 つの新しいプロファイルがセグメントの対象として認定されている場合、宛先への書き出しは増分で、5 つの新しいプロファイルのみが含まれます。

変更内容がどこにあっても、プロファイルに対してマッピングされたすべての属性が書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされたすべての属性が書き出されます。

## 書き出されたデータ {#exported-data}

エクスポート済み [!DNL Experience Platform] データは次の場所に配置されます： [!DNL Amazon Kinesis] JSON 形式で書き出します。 例えば、以下のイベントには、特定のセグメントに適合し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性が含まれます。 この見込み客の ID は、ECID と電子メールです。

```json
{
  "person": {
    "birthDate": "YYYY-MM-DD",
    "name": {
      "firstName": "John",
      "lastName": "Doe"
    }
  },
  "personalEmail": {
    "address": "john.doe@acme.com"
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
>* [Amazon Kinesisに接続し、フローサービス API を使用してデータをアクティブ化する](../../api/streaming-destinations.md)
>* [Azure イベントハブの宛先](./azure-event-hubs.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)

