---
title: Amazon Kinesis Source コネクタの概要
description: API またはユーザーインターフェイスを使用してAmazon Kinesis をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: b71fc922-7722-4279-8fc6-e5d7735e1ebb
source-git-commit: bad1e0a9d86dcce68f1a591060989560435070c5
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 12%

---

# [!DNL Amazon Kinesis] ソース

>[!IMPORTANT]
>
>- Real-Time CDP Ultimateを購入したユーザーは、ソースカタログで [!DNL Amazon Kinesis] ソースを利用できます。
>
>- Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Amazon Kinesis] ソースを使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。


Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーとのネイティブ接続を提供します。 これらのシステムから [!DNL Experience Platform] にデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータを [!DNL Experience Platform] に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。[!DNL Experience Platform] を使用すると、[!DNL Amazon Kinesis] からリアルタイムにデータを取り込むことができます。

>[!NOTE]
>
>大量のデータを取り込む必要がある場合は、[!DNL Kinesis] のスケールファクターを増やす必要があります。 現在、[!DNL Kinesis] アカウントからExperience Platformに取り込めるデータの最大量は、1 秒あたり 4,000 レコードです。 大量のデータをスケールアップして取り込むには、Adobe担当者にお問い合わせください。

## 前提条件

次の節では、[!DNL Kinesis] ソース接続を作成する前に必要な前提条件の設定について詳しく説明します。

### アクセスポリシーの設定

[!DNL Kinesis] ストリームでソース接続を作成するには、次の権限が必要です。

- `GetShardIterator`
- `GetRecords`
- `DescribeStream`
- `ListStreams`

これらの権限は [!DNL Kinesis] コンソールで整理され、資格情報を入力してデータストリームを選択すると、Experience Platformによって確認されます。

次の例に、[!DNL Kinesis] ソース接続の作成に必要な最小アクセス権を示します。

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
| `kinesis:GetShardIterator` | レコードをトラバースするために必要なアクション。 |
| `kinesis:GetRecords` | 特定のオフセットまたはシャード ID からレコードを取得するために必要なアクション。 |
| `kinesis:DescribeStream` | シャード ID を生成するために必要な、シャードマップを含むストリームに関する情報を返すアクション。 |
| `kinesis:ListStreams` | UI から選択できる、使用可能なストリームをリストするために必要なアクション。 |

[!DNL Kinesis] データストリームのアクセス制御について詳しくは、次の [[!DNL Kinesis]  ドキュメント ](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html) を参照してください。

### イテレータータイプの設定

[!DNL Kinesis] では、データの読み取り順序を指定できる次のイテレータタイプをサポートしています。

| イテレータータイプ | 説明 |
| ------------- | ----------- |
| `AT_SEQUENCE_NUMBER` | 特定のシーケンス番号で識別される位置からデータを読み出す。 |
| `AFTER_SEQUENCE_NUMBER` | データは、特定のシーケンス番号で識別される位置の後から読み出される。 |
| `AT_TIMESTAMP` | データは、特定のタイムスタンプで識別される位置から読み取られます。 |
| `TRIM_HORIZON` | データは、最も古いデータレコードから読み取られます。 |
| `LATEST` | データは、最新のデータレコードから読み取られます。 |

[!DNL Kinesis] UI ソースは現在 `TRIM_HORIZON` のみをサポートしていますが、API は `TRIM_HORIZON` と `LATEST` の両方をデータを取得するモードとしてサポートしています。 Experience Platformが [!DNL Kinesis] ソースに使用するデフォルトのイテレータ値は `TRIM_HORIZON` です。

イテレータタイプについて詳しくは、次の [[!DNL Kinesis]  ドキュメント ](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_GetShardIterator.html#API_GetShardIterator_RequestSyntax) を参照してください。

## [!DNL Amazon Kinesis] の [!DNL Experience Platform] への接続

>[!NOTE]
>
>ストリーミングデータフローを作成または更新した後、データ損失やデータ削除が発生する可能性を防ぐために、データ取り込みを 5 分間ほど一時停止する必要があります。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Amazon Kinesis] を [!DNL Experience Platform] に接続する方法について説明しています。

### API の使用

- [Flow Service API を使用したAmazon Kinesis ソース接続の作成](../../tutorials/api/create/cloud-storage/kinesis.md)
- [Flow Service API を用いたストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UI でのAmazon Kinesis ソースコネクタの作成](../../tutorials/ui/create/cloud-storage/kinesis.md)
- [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
