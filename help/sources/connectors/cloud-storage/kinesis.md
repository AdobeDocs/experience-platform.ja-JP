---
keywords: Experience Platform；ホーム；人気の高いトピック；Amazon Kinesis;amazon kinesis;Kinesis;kinesis
solution: Experience Platform
title: Amazon Kinesis Source Connector の概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用してAmazon KinesisをAdobe Experience Platformに接続する方法を説明します。
exl-id: b71fc922-7722-4279-8fc6-e5d7735e1ebb
source-git-commit: 5f4355a9d3ef39ee63581fc70dbf0f6e7d674814
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 1%

---

# [!DNL Amazon Kinesis] コネクタ

Adobe Experience Platformは、AWSなどのクラウドプロバイダーにネイティブの接続を提供します。 [!DNL Google Cloud Platform]、および [!DNL Azure]. これらのシステムからにデータを取り込むことができます。 [!DNL Platform].

クラウドストレージソースは、独自のデータを [!DNL Platform] をダウンロード、フォーマット、アップロードする必要はありません。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順が、ソースワークフローに統合されます。 [!DNL Platform] を使用すると、次のデータを取り込むことができます： [!DNL Amazon Kinesis] リアルタイムで。

>[!NOTE]
>
>の尺度 [!DNL Kinesis] 大量のデータを取り込む必要がある場合は、を増やす必要があります。 現在、 [!DNL Kinesis] Platform へのアカウントは、1 秒あたり 4,000 レコードです。 大量のデータをスケールアップして取り込むには、Adobe担当者にお問い合わせください。

## 前提条件

次の節では、を作成する前に必要な前提条件の設定について詳しく説明します [!DNL Kinesis] ソース接続。

### アクセスポリシーを設定する

A [!DNL Kinesis] ストリームでソース接続を作成するには、次の権限が必要です。

- `GetShardIterator`
- `GetRecords`
- `DescribeStream`
- `ListStreams`

これらの権限は、 [!DNL Kinesis] コンソールにアクセスし、資格情報を入力してデータストリームを選択すると、Platform によって確認されます。

次の例は、 [!DNL Kinesis] ソース接続。

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kinesis:GetShardIterator",
                "kinesis:GetRecords",
                "kinesis:DescribeStream",
                "kinesis:ListStreams"
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
| `kinesis:GetShardIterator` | レコードをトラバースするのに必要なアクション。 |
| `kinesis:GetRecords` | 特定のオフセット ID または共有 ID からレコードを取得するために必要なアクション。 |
| `kinesis:DescribeStream` | シャード ID の生成に必要なシャードマップを含む、ストリームに関する情報を返すアクション。 |
| `kinesis:ListStreams` | UI から選択できる使用可能なストリームのリストアウトに必要なアクション。 |

のアクセス制御の詳細 [!DNL Kinesis] データストリームについては、次を参照してください。 [[!DNL Kinesis] 文書](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html).

### イテレータータイプを設定

[!DNL Kinesis] では、次のイテレータータイプをサポートしており、データの読み取り順序を指定できます。

| 反復子タイプ | 説明 |
| ------------- | ----------- |
| `AT_SEQUENCE_NUMBER` | データは、特定のシーケンス番号で識別された位置から読み出される。 |
| `AFTER_SEQUENCE_NUMBER` | データは、特定のシーケンス番号で識別される位置の後から読み出される。 |
| `AT_TIMESTAMP` | データは、特定のタイムスタンプで識別された位置から読み出される。 |
| `TRIM_HORIZON` | データは、最も古いデータレコードから読み取られます。 |
| `LATEST` | データは、最新のデータレコードから読み取られます。 |

A [!DNL Kinesis] UI ソースは現在、 `TRIM_HORIZON`API は `TRIM_HORIZON` および `LATEST` をデータを取得するモードとして使用します。 Platform が [!DNL Kinesis] ソースは `TRIM_HORIZON`.

反復子のタイプの詳細については、次を参照してください。 [[!DNL Kinesis] 文書](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_GetShardIterator.html#API_GetShardIterator_RequestSyntax).

## 接続 [!DNL Amazon Kinesis] から [!DNL Platform]

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Amazon Kinesis] から [!DNL Platform] API またはユーザーインターフェイスを使用する場合：

### API の使用

- [フローサービス API を使用したAmazon Kinesisソース接続の作成](../../tutorials/api/create/cloud-storage/kinesis.md)
- [フローサービス API を使用したストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UI でのAmazon Kinesisソース接続の作成](../../tutorials/ui/create/cloud-storage/kinesis.md)
- [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
