---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Batch Ingestionの概要
topic: overview
translation-type: tm+mt
source-git-commit: 79466c78fd78c0f99f198b11a9117c946736f47a

---


# バッチ取り込みの概要

Batch Ingestion APIを使用すると、データをバッチファイルとしてAdobe Experience Platformに取り込むことができます。 取り込まれるデータは、CRMシステムのフラットファイル（パーケットファイルなど）のプロファイルデータ、またはエクスペリエンスデータモデル(XDM)レジストリの既知のスキーマに適合するデータです。

データ [取り込みAPIリファレンスは](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) 、これらのAPI呼び出しに関する追加情報を提供します。

次の図に、バッチ取り込みプロセスの概要を示します。

![](../images/batch-ingestion/overview/batch_ingestion.png)

## APIの使用

Data Ingest APIを使用すると、次の3つの基本的な手順で、データをバッチ（1つのユニットとして取り込む1つ以上のファイルで構成されるデータの単位）としてExperience Platformに取り込むことができます。

1. 新しいバッチを作成します。
2. データのXDMデータと一致する指定したデータセットにファイルをスキーマします。
3. バッチの終了を伝えます。


### データ取り込みの前提条件

- アップロードするデータは、ParquetまたはJSON形式である必要があります。
- Catalogサービスで作成されたデ [ータセット](../../catalog/home.md)。
- パーケットファイルの内容は、アップロード先のデータセットのスキーマのサブセットと一致する必要があります。
- 認証後に固有のアクセストークンを取得します。

### バッチ取り込みのベストプラクティス

- 推奨されるバッチサイズは256 MBから100 GBです。
- 各バッチには、最大1500個のファイルを含める必要があります。

512 MBを超えるファイルをアップロードするには、ファイルを小さなチャンクに分割する必要があります。 大きなファイルのアップロード手順は、こちらを参照して [ください](#large-file-upload---create-file)。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

### バッチの作成

データセットにデータを追加する前に、データをバッチにリンクする必要があります。バッチは、後で指定したデータセットにアップロードされます。

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
| `id` | 作成された（後続のリクエストで使用される）バッチのID。 |
| `relatedObjects.id` | ファイルのアップロード先のデータセットのID。 |

## ファイルのアップロード

アップロード用の新しいバッチが正常に作成されたら、ファイルを特定のデータセットにアップロードできます。

ファイルのアップロードは、小さいファ **イルアップロードAPIを使用して行いま**&#x200B;す。 ただし、ファイルが大きすぎて、ゲートウェイの制限（拡張タイムアウト、本文サイズの要求の超過、その他の制限など）を超えた場合は、 **Large File Upload APIに切り替えることができます**。 このAPIは、ファイルをチャンク単位でアップロードし、 **Large File Upload Complete API呼び出しを使用してデータをまとめ** ます。

>[!NOTE] 次の例では、parquetファイル [形式](https://parquet.apache.org/documentation/latest/) を使用します。 JSONファイル形式の使用例については、バッチインジェスト開発ガイド [を参照してください](./api-overview.md)。

### 小さいファイルのアップロード

バッチが作成されると、データを既存のデータセットにアップロードできます。  アップロードするファイルは、参照されているXDMスキーマと一致する。

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

### 大きなファイルのアップロード — ファイルの作成

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

### 大きなファイルのアップロード — 後続のパーツのアップロード

ファイルの作成後は、ファイルの各セクションに対して1つずつ、繰り返しPATCHリクエストを行うことで、以降のすべてのチャンクをアップロードできます。

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

すべてのファイルがバッチにアップロードされたら、バッチの完了を通知できます。 これにより、完了したファイルに対し **てCatalog DataSetFile** エントリが作成され、上で生成したバッチに関連付けられます。 次に、カタログバッチが成功とマークされ、ダウンストリームフローがトリガーされ、使用可能なデータが取り込まれます。

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

## バッチステータスの確認

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

このフ `"status"` ィールドは、リクエストされたバッチの現在のステータスを示します。 バッチは、次のいずれかの状態になります。

## バッチ取り込みのステータス

| Status | 説明 |
| ------ | ----------- |
| 破棄 | バッチは、期待された期間内に完了していません。 |
| 中止 | 指定したバッチに対し **て** 、（Batch Ingest APIを介して）中止操作が明示的に呼び出されました。 バッチが「読み込み済み」状態にな **ると** 、中止することはできません。 |
| アクティブ | バッチは正常にプロモーションされ、ダウンストリーム消費に使用できます。 このステータスは、成功と同じ意味で使用で **きます**。 |
| 削除済み | バッチのデータは完全に削除されました。 |
| 失敗 | 不良な設定または不正なデータ、あるいはその両方から生じる端末の状態。 失敗したバッチのデータは表 **示され** ません。 このステータスは、「失敗」と同じ意味で使用で **きます**。 |
| 非アクティブ | バッチは正常に昇格されましたが、元に戻されたか、有効期限が切れています。 バッチは、下位消費には使用できなくなります。 |
| ロード済み | バッチのデータが完了し、バッチはプロモーションの準備ができています。 |
| ロード中 | このバッチのデータはアップロード中で、バッチは現在 **プロモー** トの準備ができていません。 |
| 再試行中 | このバッチのデータは処理中です。 ただし、システムエラーまたは一時的なエラーが原因で、バッチが失敗し、その結果、このバッチは再試行中です。 |
| ステージ | バッチのプロモーションプロセスのステージング段階が完了し、インジェストジョブが実行されました。 |
| ステージング | バッチのデータは処理中です。 |
| 停止 | バッチのデータは処理中です。 ただし、バッチプロモーションは多数の再試行後に停止しました。 |