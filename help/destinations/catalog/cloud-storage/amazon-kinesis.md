---
keywords: Amazon Kinesis;kinesis destination;kinesis
title: Amazon Kinesis接続
description: Amazon Kinesisストレージへのリアルタイムアウトバウンド接続を作成し、Adobe Experience Platformからデータをストリーミングします。
exl-id: b40117ef-6ad0-48a9-bbcb-97c6f6d1dce3
source-git-commit: b1945d42b82b549985d848071762fa6ee2451368
workflow-type: tm+mt
source-wordcount: '1297'
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

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

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

Experience Platformにより、 [!DNL Amazon Kinesis] 宛先：セグメントの選定やその他の重要なイベントに従ってプロファイルに関連する更新が発生した場合にのみ、宛先にデータを書き出します。 プロファイルは、次の状況で宛先に書き出されます。

* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つのセグメントのセグメントメンバーシップの変更によって決定されました。 例えば、プロファイルは、宛先にマッピングされたいずれかのセグメントに適合しているか、宛先にマッピングされたいずれかのセグメントから退出しています。
* プロファイルの更新は、 [id マップ](/help/xdm/field-groups/profile/identitymap.md). 例えば、宛先にマッピングされたセグメントの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた属性の少なくとも 1 つに対する属性の変更によって決定されました。 例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合、関連する更新がおこなわれたプロファイルのみが宛先にエクスポートされます。 例えば、宛先フローにマッピングされたセグメントのメンバーが 100 人で、5 つの新しいプロファイルがセグメントの対象として認定されている場合、宛先への書き出しは増分で、5 つの新しいプロファイルのみが含まれます。

変更内容がどこにあっても、プロファイルに対してマッピングされたすべての属性が書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされたすべての属性が書き出されます。

### 何がデータエクスポートを決定し、何がエクスポートに含まれるか {#what-determines-export-what-is-included}

特定のプロファイルに対して書き出されるデータに関しては、 *が、 [!DNL Amazon Kinesis] 宛先* および *エクスポートに含まれるデータ*.

| 宛先の書き出しを決定する要素 | 宛先の書き出しに含まれる内容 |
|---------|----------|
| <ul><li>マッピングされた属性とセグメントは、宛先の書き出しのキューとして機能します。 つまり、マッピングされたセグメントの状態（null から適合済み、適合済み/既存から既存へ）が変更されたり、マッピングされた属性が更新された場合、宛先の書き出しがキックオフされます。</li><li>ID は現在にマッピングできないので [!DNL Amazon Kinesis] 宛先、特定のプロファイル上の ID の変更によって、宛先の書き出しも決まります。</li><li>属性に対する変更は、同じ値であるかどうかに関わらず、属性に対する更新として定義されます。 つまり、値自体が変更されていない場合でも、属性の上書きは変更と見なされます。</li></ul> | <ul><li>（最新のメンバーシップステータスを持つ）すべてのセグメントは、データフローにマッピングされているかどうかに関係なく、 `segmentMembership` オブジェクト。</li><li>内のすべての ID `identityMap` オブジェクトも含まれます (Experience Platformは現在、 [!DNL Amazon Kinesis] 宛先 )。</li><li>マッピングされた属性のみが宛先エクスポートに含まれます。</li></ul> |

{style=&quot;table-layout:fixed&quot;}

例えば、このデータフローを [!DNL Amazon Kinesis] 宛先。この宛先では、3 つのセグメントがデータフローで選択され、4 つの属性が宛先にマッピングされます。

![Amazon Kinesis宛先のデータフロー](/help/destinations/assets/catalog/http/profile-export-example-dataflow.png)

宛先へのプロファイルエクスポートは、いずれかの *3 つのマッピングされたセグメント*. ただし、データエクスポートでは、 `segmentMembership` オブジェクト ( [書き出されたデータ](#exported-data) の節を参照 )、その特定のプロファイルがそのメンバーの場合は、その他のマッピングされていないセグメントが表示されることがあります。 プロファイルが DeLorean Cars セグメントで顧客の資格を得ている一方で、「Back to the Future」映画や SF ファンセグメントのメンバーでもある場合、他の 2 つのセグメントも `segmentMembership` データエクスポートのオブジェクト（データフローでマッピングされていない場合）。

プロファイル属性の観点から、上でマッピングした 4 つの属性に対する変更によって、書き出し先が決まり、プロファイルに存在する 4 つのマッピング済み属性のいずれかがデータ書き出しに表示されます。

## 書き出されたデータ {#exported-data}

エクスポート済み [!DNL Experience Platform] データは、 [!DNL Amazon Kinesis] の宛先を JSON 形式で指定します。 例えば、以下のエクスポートには、特定のセグメントに適合し、別の 2 つのセグメントのメンバーであり、別のセグメントから離脱したプロファイルが含まれています。 書き出しには、プロファイル属性の名、姓、生年月日、個人の電子メールアドレスも含まれます。 このプロファイルの ID は、ECID と電子メールです。

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
   "ups":{
      "7841ba61-23c1-4bb3-a495-00d3g5fe1e93":{
         "lastQualificationTime":"2022-01-11T21:24:39Z",
         "status":"exited"
      },
      "59bd2fkd-3c48-4b18-bf56-4f5c5e6967ae":{
         "lastQualificationTime":"2022-01-02T23:37:33Z",
         "status":"existing"
      },
      "947c1c46-008d-40b0-92ec-3af86eaf41c1":{
         "lastQualificationTime":"2021-08-25T23:37:33Z",
         "status":"existing"
      },
      "5114d758-ce71-43ba-b53e-e2a91d67b67f":{
         "lastQualificationTime":"2022-01-11T23:37:33Z",
         "status":"realized"
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

