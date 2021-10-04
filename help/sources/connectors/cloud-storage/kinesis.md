---
keywords: Experience Platform；ホーム；人気のあるトピック；Amazon Kinesis;amazon kinesis;Kinesis;kinesis
solution: Experience Platform
title: Amazon Kinesis Source Connector の概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用してAmazon KinesisをAdobe Experience Platformに接続する方法を説明します。
exl-id: b71fc922-7722-4279-8fc6-e5d7735e1ebb
source-git-commit: af11bc966889be54fc27e02f3eee321519cef88f
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 1%

---

# [!DNL Amazon Kinesis] コネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーにネイティブ接続を提供します。 これらのシステムのデータを [!DNL Platform] に取り込むことができます。

クラウドストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを [!DNL Platform] に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で指定できます。 プロセスの各ステップは、ソースワークフローに統合されます。 [!DNL Platform] を使用すると、からデータをリアルタイム [!DNL Amazon Kinesis] で取り込むことができます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

## 前提条件

次の節では、[!DNL Kinesis] ソース接続を作成する前に必要な前提条件の設定に関する詳細を説明します。

### アクセスポリシーの設定

[!DNL Kinesis] ストリームには、ソース接続を作成するために次の権限が必要です。

- `GetShardIterator`
- `GetRecords`
- `DescribeStream`
- `ListStreams`

これらの権限は、[!DNL Kinesis] コンソールを通じて設定され、資格情報を入力してデータストリームを選択すると、Platform によって確認されます。

次の例は、[!DNL Kinesis] ソース接続を作成するために必要な最小限のアクセス権を示しています。

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
| `kinesis:GetShardIterator` | レコード間を移動するために必要なアクション。 |
| `kinesis:GetRecords` | 特定のオフセット ID または共有 ID からレコードを取得するために必要なアクション。 |
| `kinesis:DescribeStream` | シャードマップを含むストリームに関する情報を返すアクション。シャード ID の生成に必要です。 |
| `kinesis:ListStreams` | UI から選択できる使用可能なストリームのリストアウトに必要なアクション。 |

[!DNL Kinesis] データストリームのアクセス制御の詳細については、次の [[!DNL Kinesis]  ドキュメント ](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html) を参照してください。

### イテレータ型の設定

[!DNL Kinesis] では、次のイテレータ型をサポートしており、データの読み取り順序を指定できます。

| 反復子の型 | 説明 |
| ------------- | ----------- |
| `AT_SEQUENCE_NUMBER` | データは、特定のシーケンス番号で識別された位置から読み出される。 |
| `AFTER_SEQUENCE_NUMBER` | データは、特定のシーケンス番号で識別された位置から読み出される。 |
| `AT_TIMESTAMP` | データは、特定のタイムスタンプで識別された位置から読み出される。 |
| `TRIM_HORIZON` | データは、最も古いデータレコードから読み取られます。 |
| `LATEST` | データは、最新のデータレコードから読み取られます。 |

現在、[!DNL Kinesis] UI ソースは `TRIM_HORIZON` のみをサポートしていますが、API はデータを取得するモードとして、`TRIM_HORIZON` と `LATEST` の両方をサポートしています。 Platform が [!DNL Kinesis] ソースに使用するデフォルトのイテレータ値は `TRIM_HORIZON` です。

イテレータ型の詳細は、次の [[!DNL Kinesis] document](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_GetShardIterator.html#API_GetShardIterator_RequestSyntax) を参照してください。

## [!DNL Amazon Kinesis] を [!DNL Platform] に接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Amazon Kinesis] を [!DNL Platform] に接続する方法について説明します。

### API の使用

- [フローサービス API を使用したAmazon Kinesisソース接続の作成](../../tutorials/api/create/cloud-storage/kinesis.md)
- [フローサービス API を使用したストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UI でのAmazon Kinesisソース接続の作成](../../tutorials/ui/create/cloud-storage/kinesis.md)
- [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
