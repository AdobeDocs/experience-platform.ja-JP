---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformバッチ取り込みの概要
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '1170'
ht-degree: 2%

---


# バッチ取り込みの概要

Batch Ingestion APIを使用すると、データをバッチファイルとしてAdobe Experience Platformに取り込むことができます。 取り込まれるデータは、CRMシステムのフラットファイル（パーケーファイルなど）のプロファイルデータ、またはエクスペリエンスデータモデル(XDM)レジストリの既知のスキーマに準拠するデータです。

[データ取り込みAPIリファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) は、これらのAPI呼び出しに関する追加情報を提供します。

次の図に、バッチインジェスト処理の概要を示します。

![](../images/batch-ingestion/overview/batch_ingestion.png)

## APIの使用

Data Ingestion APIを使用すると、次の3つの基本的な手順でデータをバッチ（1つのユニットとして取り込まれる1つ以上のファイルで構成されるデータの単位）としてExperience Platformに取り込むことができます。

1. 新しいバッチを作成します。
2. データのXDMスキーマに一致する指定したデータセットにファイルをアップロードします。
3. バッチの終わりを伝えます。


### データ取り込みの前提条件

- アップロードするデータは、ParketまたはJSON形式である必要があります。
- [Catalogサービスで作成されたデータセット](../../catalog/home.md)。
- パーケットファイルの内容は、アップロード先のデータセットのスキーマのサブセットと一致する必要があります。
- 認証後に固有のアクセストークンを持つ。

### バッチ取り込みのベストプラクティス

- 推奨されるバッチサイズは256 MB ～ 100 GBです。
- 各バッチには、最大1500個のファイルを含める必要があります。

512 MBを超えるファイルをアップロードするには、ファイルをより小さなチャンクに分割する必要があります。 大きなファイルのアップロード手順は、 [こちらを参照してください](#large-file-upload---create-file)。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform内のサンドボックスについて詳しくは、「 [Sandboxの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

### バッチの作成

データをデータセットに追加する前に、データをバッチにリンクする必要があります。バッチは後で指定したデータセットにアップロードされます。

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
  -H "x-api-key : {API_KEY}"
  -d '{ 
          "datasetId": "{DATASET_ID}" 
      }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `datasetId` | ファイルのアップロード先のデータセットのID。 |

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
| `id` | 作成されたばかりのバッチのID（以降のリクエストで使用）。 |
| `relatedObjects.id` | ファイルのアップロード先のデータセットのID。 |

## ファイルのアップロード

アップロード用の新しいバッチが正常に作成されたら、ファイルを特定のデータセットにアップロードできます。

ファイルは、 **小さいファイルアップロードAPIを使用してアップロードできます**。 ただし、ファイルが大きすぎて、ゲートウェイの制限を超えている場合（拡張タイムアウト、本文のサイズの要求の超過、その他の制限など）は、 **Large File Upload API**（大きいファイルアップロードAPI）に切り替えることができます。 このAPIは、ファイルをチャンク単位でアップロードし、 **Large File Upload Complete API** 呼び出しを使用してデータをまとめます。

>[!NOTE]
>
>次の例では、パーケット [ファイル形式を使用し](https://parquet.apache.org/documentation/latest/) ます。 JSONファイル形式の使用例については、『 [batch ingestion developer guide](./api-overview.md)』を参照してください。

### 小さいファイルのアップロード

バッチを作成すると、データを既存のデータセットにアップロードできます。  アップロードするファイルは、参照されているXDMスキーマと一致する必要があります。

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | バッチのID。 |
| `{DATASET_ID}` | ファイルをアップロードするデータセットのID。 |
| `{FILE_NAME}` | データセットに表示されるファイルの名前。 |

**リクエスト**

```SHELL
curl -X PUT "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet" \
  -H "content-type: application/octet-stream" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}" \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_PATH_AND_NAME}` | データセットにアップロードするファイルのパスとファイル名。 |

**レポンス**

```JSON
#Status 200 OK, with empty response body
```

### 大きいファイルのアップロード — ファイルの作成

大きなファイルをアップロードするには、ファイルを小さなチャンクに分割し、一度に1つずつアップロードする必要があります。

```http
POST /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}?action=initialize
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | バッチのID。 |
| `{DATASET_ID}` | ファイルを取り込むデータセットのID。 |
| `{FILE_NAME}` | データセットに表示されるファイルの名前。 |

**リクエスト**

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/part1=a/part2=b/{FILE_NAME}.parquet?action=initialize" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

**レポンス**

```JSON
#Status 201 CREATED, with empty response body
```

### 大きいファイルのアップロード — 後続のパーツをアップロード

ファイルの作成後は、PATCHリクエストを繰り返し行うことで、以降のすべてのチャンクをアップロードできます。このリクエストは、ファイルの各セクションに対して1つずつ行われます。

```http
PATCH /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | バッチのID。 |
| `{DATASET_ID}` | ファイルのアップロード先のデータセットのID。 |
| `{FILE_NAME}` | データセットに表示されるファイルの名前。 |

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

**レポンス**

```JSON
#Status 200 OK, with empty response
```

## シグナルバッチ完了

すべてのファイルがバッチにアップロードされた後、バッチは完了を示すために署名できます。 これにより、完了したファイルに対してCatalog **DataSetFile** エントリが作成され、上で生成したバッチに関連付けられます。 次に、カタログバッチが成功とマークされ、ダウンストリームフローがトリガーされ、使用可能なデータが取り込まれます。

**リクエスト**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | データセットにアップロードするバッチのID。 |

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
-H "x-gw-ims-org-id: {IMS_ORG}" \
-H "x-sandbox-name: {SANDBOX_NAME}" \
-H "Authorization: Bearer {ACCESS_TOKEN}" \
-H "x-api-key : {API_KEY}"
```

**レポンス**

```JSON
#Status 200 OK, with empty response
```

## バッチ状態の確認

バッチにファイルがアップロードされるのを待つ間、バッチのステータスをチェックして進行状況を確認できます。

**API形式**

```http
GET /batch/{BATCH_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | チェックするバッチのID。 |

**リクエスト**

```shell
curl GET "https://platform.adobe.io/data/foundation/catalog/batch/{BATCH_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

**レポンス**

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
| `{USER_ID}` | バッチを作成または更新したユーザーのID。 |

この `"status"` フィールドには、要求されたバッチの現在のステータスが表示されます。 バッチは、次のいずれかの状態にできます。

## バッチ取り込みのステータス

| Status | 説明 |
| ------ | ----------- |
| 破棄 | バッチは、期待された時間枠内に完了していません。 |
| 中止 | 中止操作が、指定されたバッチに対して **明示的に** （バッチ取り込みAPIを介して）呼び出されました。 バッチが **Loaded** 状態になると、中止できなくなります。 |
| アクティブ | バッチは正常にプロモートされ、ダウンストリーム消費に使用できます。 このステータスは、 **成功と同じように使用できます**。 |
| 削除済み | バッチのデータは完全に削除されました。 |
| 失敗 | 不正な構成または不正なデータ、あるいはその両方から生じる端末状態。 失敗したバッチのデータは **表示されません** 。 このステータスは、 **失敗と同じ意味で使用できます**。 |
| 非アクティブ | バッチは正常にプロモートされましたが、元に戻されたか、期限が切れています。 バッチは、下流消費には使用できなくなります。 |
| ロード済み | バッチのデータが完了し、バッチはプロモーションの準備ができています。 |
| ロード中 | このバッチのデータはアップロード中で、バッチは現在 **** 、昇格する準備ができていません。 |
| 再試行中 | このバッチのデータは処理中です。 ただし、システムまたは一時的なエラーが原因で、バッチが失敗しました。その結果、このバッチは再試行中です。 |
| 段階的 | バッチのプロモーションプロセスのステージング段階が完了し、インジェストジョブが実行されました。 |
| ステージング | バッチのデータは処理中です。 |
| 停止 | バッチのデータを処理中です。 ただし、バッチプロモーションは、多数の再試行の後に停止しています。 |