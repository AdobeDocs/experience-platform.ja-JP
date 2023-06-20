---
title: Amazon Kinesis Source Connector の概要
description: API またはユーザーインターフェイスを使用してAmazon KinesisをAdobe Experience Platformに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: b71fc922-7722-4279-8fc6-e5d7735e1ebb
source-git-commit: 9a8139c26b5bb5ff937a51986967b57db58aab6c
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# [!DNL Amazon Kinesis] ソース

>[!IMPORTANT]
>
>この [!DNL Amazon Kinesis] ソースは、Real-time Customer Data Platform Ultimate を購入したユーザーがソースカタログで利用できます。

Adobe Experience Platformは、AWSなどのクラウドプロバイダーにネイティブの接続を提供します。 [!DNL Google Cloud Platform]、および [!DNL Azure]. これらのシステムからにデータを取り込むことができます。 [!DNL Platform].

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータを [!DNL Platform] に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。[!DNL Platform] では、[!DNL Amazon Kinesis] からリアルタイムにデータを取り込むことができます。

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
| `AT_TIMESTAMP` | データは、特定のタイムスタンプで識別される位置から読み出される。 |
| `TRIM_HORIZON` | データは、最も古いデータレコードから読み取られます。 |
| `LATEST` | データは、最新のデータレコードから読み取られます。 |

A [!DNL Kinesis] UI ソースは現在、 `TRIM_HORIZON`API は `TRIM_HORIZON` および `LATEST` をデータを取得するモードとして使用します。 Platform が [!DNL Kinesis] ソースは `TRIM_HORIZON`.

反復子のタイプの詳細については、次を参照してください。 [[!DNL Kinesis] 文書](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_GetShardIterator.html#API_GetShardIterator_RequestSyntax).

## 接続 [!DNL Amazon Kinesis] から [!DNL Platform]

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Amazon Kinesis] と を接続する方法について説明します。[!DNL Platform]

### API の使用

- [フローサービス API を使用したAmazon Kinesisソース接続の作成](../../tutorials/api/create/cloud-storage/kinesis.md)
- [Flow Service API を用いたストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UI でのAmazon Kinesisソース接続の作成](../../tutorials/ui/create/cloud-storage/kinesis.md)
- [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
