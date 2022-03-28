---
keywords: Experience Platform；ホーム；人気の高いトピック；データ取得；バッチ；バッチ；データセットの有効化；バッチ取得の概要；概要；バッチ取得の概要；
solution: Experience Platform
title: バッチ取得 API の概要
topic-legacy: overview
description: Adobe Experience Platform Data Ingest API を使用すると、データをバッチファイルとして Platform に取り込むことができます。 CRM システムのフラットファイルのプロファイルデータ（Parquet ファイルなど）、または Experience Data Model(XDM) レジストリの既知のスキーマに適合するデータを取り込むことができます。
exl-id: ffd1dc2d-eff8-4ef7-a26b-f78988f050ef
source-git-commit: 75426b1ddc16af39eb6c423027fac7d4d0e21c6a
workflow-type: tm+mt
source-wordcount: '1387'
ht-degree: 71%

---

# バッチ取得 API の概要

Adobe Experience Platform Data Ingest API を使用すると、データをバッチファイルとして Platform に取り込むことができます。 フラットファイル（Parquet ファイルなど）のプロファイルデータ、または [!DNL Experience Data Model] (XDM) レジストリ。

[データ取得 API のリファレンスは](https://www.adobe.io/experience-platform-apis/references/data-ingestion/)では、これらの API 呼び出しに関する追加情報が提供されています。

次の図に、バッチインジェストプロセスの概要を示します。

![](../images/batch-ingestion/overview/batch_ingestion.png)

## はじめに

このガイドで使用される API エンドポイントは、 [データ取得 API](https://www.adobe.io/experience-platform-apis/references/data-ingestion/). 続行する前に、 [入門ガイド](getting-started.md) 関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方のガイド、および任意のExperience PlatformAPI を正しく呼び出すために必要な必須ヘッダーに関する重要な情報。

### [!DNL Data Ingestion] 前提条件

- アップロードするデータは、Parquet 形式または JSON 形式である必要があります。
- データセットを [[!DNL Catalog services]](../../catalog/home.md).
- Parquet ファイルの内容は、アップロード先のデータセットのスキーマのサブセットと一致する必要があります。
- 認証後に固有のアクセストークンを取得する必要があります。

### バッチインジェストのベストプラクティス

- 推奨されるバッチサイズは 256 MB～100 GB です。
- 各バッチには、最大 1,500 個のファイルを含めることができます。

### バッチ取り込みの制約

バッチデータ取得には、いくつかの制約があります。

- バッチあたりの最大ファイル数：1500
- 最大バッチサイズ：100GB
- 1 行あたりのプロパティまたはフィールドの最大数：10000
- 1 ユーザーあたりの 1 分あたりの最大バッチ数：138

>[!NOTE]
>
>512 MB を超えるファイルをアップロードする場合は、ファイルを小さなチャンクに分割する必要があります。大きなファイルをアップロードする手順については、 [このドキュメントの大きなファイルのアップロードの節](#large-file-upload---create-file).

### タイプ

データを取り込む際は、次の点を理解することが重要です。 [!DNL Experience Data Model] (XDM) スキーマが機能します。 XDM のフィールドタイプを様々な形式にマップする方法について詳しくは、『[スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md)』を参照してください。

データ取得には柔軟性があります。ターゲットスキーマ内のデータとタイプが一致しない場合、データは表現されたターゲットタイプに変換されます。  できない場合は、バッチが `TypeCompatibilityException` で失敗します。

例えば、JSON も CSV も `date` または `date-time` タイプ。 その結果、これらの値は [ISO 8061 形式の文字列](https://www.iso.org/iso-8601-date-and-time-format.html) (&quot;2018-07-10T15:05:59.000～08:00&quot;) または Unix 時間 ( ミリ秒 (1531263959000) で書式設定され、取得時にターゲット XDM タイプに変換されます。

次の表に、データの取得時にサポートされる変換を示します。

| 受信（行）とターゲット（列） | String | Byte | Short | Integer | Long | Double | Date | Date-Time | Object | Map |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 文字列 | X | X | X | X | X | X | X | X |  |  |
| Byte | X | X | X | X | X | X |  |  |  |  |
| Short | X | X | X | X | X | X |  |  |  |  |
| 整数 | X | X | X | X | X | X |  |  |  |  |
| Long | X | X | X | X | X | X | X | X |  |  |
| ダブル | X | X | X | X | X | X |  |  |  |  |
| 日付 |  |  |  |  |  |  | X |  |  |  |
| Date-Time |  |  |  |  |  |  |  | X |  |  |
| オブジェクト |  |  |  |  |  |  |  |  | X | X |
| マップ |  |  |  |  |  |  |  |  | X | X |

>[!NOTE]
>
> ブール値と配列は他の型に変換できません。

## API の使用

この [!DNL Data Ingestion] API を使用すると、データをバッチ（1 つのユニットとして取り込む 1 つ以上のファイルで構成されるデータの単位）としてに取り込むことができます。 [!DNL Experience Platform] 3 つの基本的な手順を次に示します。

1. 新しいバッチを作成します。
2. データの XDM スキーマと一致する、指定したデータセットにファイルをアップロードします。
3. バッチの終了を示します。

## バッチの作成

データをデータセットに追加するには、データをバッチにリンクする必要があります。その後、バッチは、指定したデータセットにアップロードされます。

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "Content-Type: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
  -d '{ 
          "datasetId": "{DATASET_ID}" 
      }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `datasetId` | ファイルのアップロード先のデータセットの ID。 |

**レポンス**

```JSON
{
    "id": "{BATCH_ID}",
    "imsOrg": "{IMS_ORG}",
    "updated": 0,
    "status": "loading",
    "created": 0,
    "relatedObjects": [
        {
            "type": "dataSet",
            "id": "{DATASET_ID}"
        }
    ],
    "version": "1.0.0",
    "tags": {},
    "createdUser": "{USER_ID}",
    "updatedUser": "{USER_ID}"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 作成されたバッチの ID（後続のリクエストで使用します）。 |
| `relatedObjects.id` | ファイルのアップロード先のデータセットの ID。 |

## ファイルのアップロード

アップロード用の新しいバッチが正常に作成されたら、ファイルを特定のデータセットにアップロードできます。

ファイルをアップロードするには、Small File Upload API を使用します。ただし、ファイルのサイズが大きすぎて、ゲートウェイの制限（拡張タイムアウト、リクエストの本文のサイズ、その他の制限）を超える場合は、Large File Upload API に切り替えることができます。この API は、ファイルをチャンク単位でアップロードし、その後 Large File Upload Complete API 呼び出しを使用してデータを統合します。

>[!NOTE]
>
>バッチ取り込みは、プロファイルストア内のデータを増分的に更新するために使用できます。 詳しくは、 [バッチの更新](#patch-a-batch) 内 [バッチ取得開発者ガイド](api-overview.md).

>[!INFO]
>
>以下の例では、 [Apache Parquet](https://parquet.apache.org/docs/) ファイル形式。 JSON ファイル形式の使用例については、『[バッチ取得開発者ガイド](api-overview.md)』を参照してください。

### サイズの小さなファイルのアップロード

バッチを作成したら、データを既存のデータセットにアップロードできます。アップロードするファイルは、参照されている XDM スキーマに一致している必要があります。

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | バッチの ID。 |
| `{DATASET_ID}` | ファイルをアップロードするデータセットの ID。 |
| `{FILE_NAME}` | データセットに表示される際のファイルの名前。 |

**リクエスト**

```SHELL
curl -X PUT "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet" \
  -H "content-type: application/octet-stream" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_PATH_AND_NAME}` | データセットにアップロードするファイルのパスとファイル名。 |

**レスポンス**

```JSON
#Status 200 OK, with empty response body
```

### サイズの大きなファイルのアップロード - ファイルの作成

サイズの大きなファイルをアップロードするには、ファイルを小さなチャンクに分割し、一度に 1 つずつアップロードする必要があります。

```http
POST /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}?action=initialize
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | バッチの ID。 |
| `{DATASET_ID}` | ファイルを取り込むデータセットの ID。 |
| `{FILE_NAME}` | データセットに表示される際のファイルの名前。 |

**リクエスト**

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/part1=a/part2=b/{FILE_NAME}.parquet?action=initialize" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

**レスポンス**

```JSON
#Status 201 CREATED, with empty response body
```

### サイズの大きなファイルのアップロード - 後続チャンクのアップロード

ファイルを作成したら、ファイルの各セクションに対して 1 回ずつ、PATCH リクエストを繰り返しおこなうことで、以降のすべてのチャンクをアップロードできます。

```http
PATCH /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | バッチの ID。 |
| `{DATASET_ID}` | ファイルのアップロード先のデータセットの ID。 |
| `{FILE_NAME}` | データセットに表示される際のファイルの名前。 |

**リクエスト**

```SHELL
curl -X PATCH "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/part1=a/part2=b/{FILE_NAME}.parquet" \
  -H "content-type: application/octet-stream" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -H "Content-Range: bytes {CONTENT_RANGE}" \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_PATH_AND_NAME}` | データセットにアップロードするファイルのパスとファイル名。 |

**レスポンス**

```JSON
#Status 200 OK, with empty response
```

## バッチ完了を示す

すべてのファイルをバッチにアップロードしたら、バッチの完了を示すことができます。これを行うと、 [!DNL Catalog] DataSetFile エントリは、完了したファイルに対して作成され、上で生成したバッチに関連付けられます。 これにより、[!DNL Catalog] のバッチが成功とマークされ、ダウンストリームフローがトリガーされて使用可能なデータを取り込みます。

**リクエスト**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | データセットにアップロードするバッチの ID。 |

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
-H "x-gw-ims-org-id: {IMS_ORG}" \
-H "x-sandbox-name: {SANDBOX_NAME}" \
-H "Authorization: Bearer {ACCESS_TOKEN}" \
-H "x-api-key: {API_KEY}"
```

**レスポンス**

```JSON
#Status 200 OK, with empty response
```

## バッチステータスの確認

バッチにファイルがアップロードされるのを待つ間、バッチのステータスを確認して進行状況を確認できます。

**API 形式**

```http
GET /batch/{BATCH_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 確認するバッチの ID。 |

**リクエスト**

```shell
curl GET "https://platform.adobe.io/data/foundation/catalog/batch/{BATCH_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

**レスポンス**

```JSON
{
    "{BATCH_ID}": {
        "imsOrg": "{IMS_ORG}",
        "created": 1494349962314,
        "createdClient": "MCDPCatalogService",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "updated": 1494349963467,
        "externalId": "{EXTERNAL_ID}",
        "status": "success",
        "errors": [
            {
                "code": "err-1494349963436"
            }
        ],
        "version": "1.0.3",
        "availableDates": {
            "startDate": 1337,
            "endDate": 4000
        },
        "relatedObjects": [
            {
                "type": "batch",
                "id": "foo_batch"
            },
            {
                "type": "connection",
                "id": "foo_connection"
            },
            {
                "type": "connector",
                "id": "foo_connector"
            },
            {
                "type": "dataSet",
                "id": "foo_dataSet"
            },
            {
                "type": "dataSetView",
                "id": "foo_dataSetView"
            },
            {
                "type": "dataSetFile",
                "id": "foo_dataSetFile"
            },
            {
                "type": "expressionBlock",
                "id": "foo_expressionBlock"
            },
            {
                "type": "service",
                "id": "foo_service"
            },
            {
                "type": "serviceDefinition",
                "id": "foo_serviceDefinition"
            }
        ],
        "metrics": {
            "foo": 1337
        },
        "tags": {
            "foo_bar": [
                "stuff"
            ],
            "bar_foo": [
                "woo",
                "baz"
            ],
            "foo/bar/foo-bar": [
                "weehaw",
                "wee:haw"
            ]
        },
        "inputFormat": {
            "format": "parquet",
            "delimiter": ".",
            "quote": "`",
            "escape": "\\",
            "nullMarker": "",
            "header": "true",
            "charset": "UTF-8"
        }
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{USER_ID}` | バッチを作成または更新したユーザーの ID。 |

`"status"` フィールドに、リクエストしたバッチの現在のステータスが示されます。バッチの状態は次のいずれかです。

## バッチインジェストのステータス

| ステータス | 説明 |
| ------ | ----------- |
| Abandoned | バッチは、予想期間内に完了しませんでした。 |
| Aborted | 指定したバッチに対して、中止操作が&#x200B;**明示的に**（Batch Ingest API を介して）呼び出されました。バッチの状態が「Loaded」の場合、バッチを中止することはできません。 |
| アクティブ | バッチは正常に昇格しており、ダウンストリームで使用することができます。このステータスは、「Success」と同じ意味で使用できます。 |
| Deleted | バッチのデータは完全に削除されました。 |
| Failed | 設定またはデータ、あるいはその両方が不正なために発生した、失敗の状態。失敗したバッチのデータは、表示&#x200B;**されません**。このステータスは、「Failure」と同じ意味で使用できます。 |
| Inactive | バッチは正常に昇格しましたが、元に戻されたか、有効期限が切れています。バッチは、ダウンストリームで使用できません。 |
| Loaded | バッチのデータのアップロードが完了し、バッチ昇格の準備ができています。 |
| Loading | このバッチのデータはアップロード中です。現在、バッチ昇格の準備はできて&#x200B;**いません**。 |
| Retrying | このバッチのデータは処理中です。ただし、システムエラーまたは一時的なエラーが原因でバッチが失敗しました。その結果、このバッチは再試行中です。 |
| Staged | バッチの昇進プロセスのステージング段階が完了し、インジェストジョブが実行されました。 |
| Staging | バッチのデータは処理中です。 |
| Stalled | バッチのデータは処理中です。ただし、バッチの昇進は、数回再試行した後に停止されました。 |
