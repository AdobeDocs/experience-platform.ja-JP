---
keywords: Experience Platform；ホーム；人気のあるトピック；AmazonKinesis;amazon kinesis;Kinesis;kinesis
solution: Experience Platform
title: AmazonKinesisソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用して、AmazonKinesisをAdobe Experience Platformに接続する方法を説明します。
exl-id: b71fc922-7722-4279-8fc6-e5d7735e1ebb
translation-type: tm+mt
source-git-commit: af11bc966889be54fc27e02f3eee321519cef88f
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 1%

---

# [!DNL Amazon Kinesis] コネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーに対してネイティブの接続を提供します。 これらのシステムのデータを[!DNL Platform]に取り込むことができます。

Cloudストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを[!DNL Platform]に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parket、または区切り形式で形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] データをリアルタイムで取り込むこ [!DNL Amazon Kinesis] とができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件

次の節では、[!DNL Kinesis]ソース接続を作成する前に必要な前提条件の設定に関する詳細を説明します。

### アクセスポリシーの設定

[!DNL Kinesis]ストリームは、ソース接続を作成するために次の権限が必要です。

- `GetShardIterator`
- `GetRecords`
- `DescribeStream`
- `ListStreams`

これらの権限は[!DNL Kinesis]コンソールを介して設定され、資格情報を入力してデータストリームを選択すると、プラットフォームによって確認されます。

次の例は、[!DNL Kinesis]ソース接続を作成するのに必要な最小アクセス権を示しています。

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
| `kinesis:GetShardIterator` | レコード間を移動するために必要なアクションです。 |
| `kinesis:GetRecords` | 特定のオフセットIDまたは共有IDからレコードを取得するために必要なアクション。 |
| `kinesis:DescribeStream` | 共有IDの生成に必要な共有マップを含むストリームに関する情報を返すアクション。 |
| `kinesis:ListStreams` | UIから選択できる使用可能なストリームをリストするために必要なアクション。 |

[!DNL Kinesis]データストリームのアクセス制御について詳しくは、次の[[!DNL Kinesis] ドキュメント](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html)を参照してください。

### 反復子の型を設定

[!DNL Kinesis] は、次のイテレータタイプをサポートしており、データの読み取り順序を指定できます。

| 反復子タイプ | 説明 |
| ------------- | ----------- |
| `AT_SEQUENCE_NUMBER` | 特定のシーケンス番号で識別される位置からデータの読み出しを開始する。 |
| `AFTER_SEQUENCE_NUMBER` | 特定のシーケンス番号で識別された位置からデータの読み出しを開始する。 |
| `AT_TIMESTAMP` | 特定のタイムスタンプで識別される位置からデータの読み出しを開始する。 |
| `TRIM_HORIZON` | 最も古いデータレコードからデータが読み込まれます。 |
| `LATEST` | 最新のデータレコードからデータが読み込まれます。 |

[!DNL Kinesis] UIソースは現在`TRIM_HORIZON`をサポートしていますが、APIはデータを取得するためのモードとして`TRIM_HORIZON`と`LATEST`の両方をサポートしています。 Platformが[!DNL Kinesis]ソースに対して使用するデフォルトのイテレータ値は`TRIM_HORIZON`です。

イテレーターの型について詳しくは、次の[[!DNL Kinesis] ドキュメント](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_GetShardIterator.html#API_GetShardIterator_RequestSyntax)を参照してください。

## [!DNL Amazon Kinesis]を[!DNL Platform]に接続

次のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Amazon Kinesis]を[!DNL Platform]に接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してAmazonKinesisのソース接続を作成する](../../tutorials/api/create/cloud-storage/kinesis.md)
- [Flow Service APIを使用してストリーミングデータを収集する](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UIでのAmazonKinesisソース接続の作成](../../tutorials/ui/create/cloud-storage/kinesis.md)
- [UIでのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
