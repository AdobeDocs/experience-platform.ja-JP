---
title: 外部 ML 環境へのデータの書き出し
description: Data Distillerで作成した準備済みのトレーニングデータセットを、ML 環境がモデルのトレーニングとスコアリングのために読み取ることができるクラウドストレージの場所に共有する方法を説明します。
exl-id: 75022acf-fafd-41d6-8dfa-ff3fd4c4fa7e
source-git-commit: 7cde32f841497edca7de0c995cc4c14501206b1a
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 6%

---

# 外部 ML 環境へのデータの書き出し

このドキュメントでは、Data Distillerで作成された準備済みトレーニングデータセットを、ML 環境がモデルのトレーニングとスコアリングのために読み取ることができるクラウドストレージの場所に共有する方法を説明します。 この例では、トレーニングデータセットを [ データランディングゾーン（DLZ） ](../../../sources/tutorials/api/create/cloud-storage/data-landing-zone.md) に書き出しています。 必要に応じて、機械学習環境を操作するためにストレージの宛先を変更できます。

[ 宛先用のフローサービス ](https://developer.adobe.com/experience-platform-apis/references/destinations/) は、計算された機能のデータセットを適切なクラウドストレージの場所にランディングすることで、機能パイプラインを完了するために使用されます。

## ソース接続の作成 {#create-source-connection}

ソース接続は、Adobe Experience Platform データセットへの接続を設定する役割を果たし、結果として得られるフローがデータの検索場所と形式を正確に把握できるようにします。

```python
from aepp import flowservice
flow_conn = flowservice.FlowService()

training_dataset_id = <YOUR_TRAINING_DATASET_ID>

source_res = flow_conn.createSourceConnectionDataLake(
    name=f"[CMLE] Featurized Dataset source connection created by {username}",
    dataset_ids=[training_dataset_id],
    format="parquet"
)
source_connection_id = source_res["id"]
```

## ターゲット接続の作成 {#create-target-connection}

ターゲット接続は、宛先ファイルシステムへの接続を担当します。 これには、まずクラウドストレージアカウント（この例ではデータランディングゾーン）へのベース接続を作成し、次に、指定された圧縮および形式オプションを持つ特定のファイルパスへのターゲット接続を作成する必要があります。

使用可能なクラウドストレージの宛先は、それぞれ接続仕様 ID で識別されます。

| クラウドストレージタイプ | 接続仕様 ID |
|-----------------------|--------------------------------------|
| Amazon S3 | 4fce964d-3f37-408f-9778-e597338a21ee |
| Azure Blob ストレージ | 6d6b59bf-fb58-4107-9064-4d246c0e5bb2 |
| Azure データレイク | be2c3209-53bc-47e7-ab25-145db8b873e1 |
| Data Landing Zone | 10440537-2a7b-4583-ac39-ed38d4b848e8 |
| Google Cloud Storage | c5d93acb-ea8b-4b14-8f53-02138444ae99 |
| SFTP | 36965a81-b1c6-401b-99f8-22508f1e6a26 |

```python
connection_spec_id = "10440537-2a7b-4583-ac39-ed38d4b848e8"
base_connection_res = flow_conn.createConnection(data={
    "name": "Base Connection to DLZ created by",
    "auth": None,
    "connectionSpec": {
        "id": connection_spec_id,
        "version": "1.0"
    }
})
base_connection_id = base_connection_res["id"]

target_res = flow_conn.createTargetConnection(
    data={
        "name": "Data Landing Zone target connection",
        "baseConnectionId": base_connection_id,
        "params": {
            "mode": "Server-to-server",
            "compression": config.get("Cloud", "compression_type"),
            "datasetFileType": config.get("Cloud", "data_format"),
            "path": config.get("Cloud", "export_path")
        },
        "connectionSpec": {
            "id": connection_spec_id,
            "version": "1.0"
        }
    }
)
target_connection_id = target_res["id"]
```

## データフローの作成 {#create-data-flow}

最後の手順では、ソース接続で指定されたデータセットとターゲット接続で指定された宛先ファイルパスの間でデータフローを作成します。

使用可能な各クラウドストレージタイプは、フロー仕様 ID によって識別されます。

| クラウドストレージタイプ | フロー仕様 ID |
|-----------------------|--------------------------------------|
| Amazon S3 | 269ba276-16fc-47db-92b0-c1049a3c131f |
| Azure Blob ストレージ | 95bd8965-fc8a-4119-b9c3-944c2c2df6d2 |
| Azure データレイク | 17be2013-2549-41ce-96e7-a70363bec293 |
| Data Landing Zone | cd2fc47e-e838-4f38-a581-8fff2f99b63a |
| Google Cloud Storage | 585c15c4-6cbf-4126-8f87-e26bff78b657 |
| SFTP | 354d6aad-4754-46e4-a576-1b384561c440 |

次のコードでは、遠い将来に開始するようにスケジュールが設定されたデータフローを作成します。 これにより、モデル開発中にアドホックフローをトリガー設定できます。 トレーニング済みモデルが完成したら、データフローのスケジュールを更新して、目的のスケジュールで機能データセットを共有できます。

```python
import time

on_schedule = False
if on_schedule:
    schedule_params = {
        "interval": 3,
        "timeUnit": "hour",
        "startTime": int(time.time())
    }
else:
    schedule_params = {
        "interval": 1,
        "timeUnit": "day",
        "startTime": int(time.time() + 60*60*24*365) # Start the schedule far in the future
    }

flow_spec_id = "cd2fc47e-e838-4f38-a581-8fff2f99b63a"
flow_obj = {
    "name": "Flow for Feature Dataset to DLZ",
    "flowSpec": {
        "id": flow_spec_id,
        "version": "1.0"
    },
    "sourceConnectionIds": [
        source_connection_id
    ],
    "targetConnectionIds": [
        target_connection_id
    ],
    "transformations": [],
    "scheduleParams": schedule_params
}
flow_res = flow_conn.createFlow(
    obj = flow_obj,
    flow_spec_id = flow_spec_id
)
dataflow_id = flow_res["id"]
```

データフローを作成したら、アドホックフロー実行をトリガーして、オンデマンドで機能データセットを共有できるようになりました。

```python
from aepp import connector

connector = connector.AdobeRequest(
  config_object=aepp.config.config_object,
  header=aepp.config.header,
  loggingEnabled=False,
  logger=None,
)

endpoint = aepp.config.endpoints["global"] + "/data/core/activation/disflowprovider/adhocrun"

payload = {
    "activationInfo": {
        "destinations": [
            {
                "flowId": dataflow_id, 
                "datasets": [
                    {"id": created_dataset_id}
                ]
            }
        ]
    }
}

connector.header.update({"Accept":"application/vnd.adobe.adhoc.dataset.activation+json; version=1"})
activation_res = connector.postData(endpoint=endpoint, data=payload)
activation_res
```

## データランディングゾーンへの共有を効率化

データセットをデータランディングゾーンでより簡単に共有するために、`aepp` ライブラリには、上記の手順を 1 回の関数呼び出しで実行する `exportDatasetToDataLandingZone` 関数が用意されています。

```python
from aepp import exportDatasetToDataLandingZone

export = exportDatasetToDataLandingZone.ExportDatasetToDataLandingZone()

dataflow_id = export.createDataFlowRunIfNotExists(
    dataset_id = created_dataset_id,
    data_format = data_format, 
    export_path= export_path, 
    compression_type = compression_type, 
    on_schedule = False, 
    config_path = config_path, 
    entity_name = "Flow for Featurized Dataset to DLZ"
)
```

このコードは、指定されたパラメーターに基づいてソース接続、ターゲット接続およびデータフローを作成し、データフローのアドホック実行を 1 つの手順で実行します。
