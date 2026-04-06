---
keywords: Experience Platform；ホーム；人気のトピック；バッチ取り込み；バッチ取り込み；取り込み；開発者ガイド；API ガイド；アップロード；取り込みParquet；取り込みjson;
solution: Experience Platform
title: Batch Ingestion API ガイド
description: このドキュメントでは、Adobe Experience Platformのバッチ取り込みAPIを使用する開発者向けの包括的なガイドを提供します。
exl-id: 4ca9d18d-1b65-4aa7-b608-1624bca19097
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '2435'
ht-degree: 64%

---

# バッチ取り込み開発者ガイド

このドキュメントでは、Adobe Experience Platformで[&#x200B; バッチ取り込みAPI エンドポイント &#x200B;](https://developer.adobe.com/experience-platform-apis/references/batch-ingestion/)を使用するための包括的なガイドを提供します。 前提条件やベストプラクティスを含むバッチ取得APIの概要については、[&#x200B; バッチ取得APIの概要](overview.md)を参照してください。

このドキュメントの付録では、CSV 例や JSON データファイル例など、[取り込みに使用するデータの形式設定](#data-transformation-for-batch-ingestion)に関する情報を提供します。

## はじめに

このガイドで使用されているAPI エンドポイントは、[&#x200B; バッチ取り込みAPI](https://developer.adobe.com/experience-platform-apis/references/batch-ingestion/)の一部です。 バッチ取り込みはRESTful APIを通じて提供され、サポートされているオブジェクトタイプに対して基本的なCRUD操作を実行できます。

続行する前に、[&#x200B; バッチ取り込みAPIの概要](overview.md)と[入門ガイド &#x200B;](getting-started.md)を確認してください。

## JSON ファイルの取得

>[!NOTE]
>
>- 次の手順は、小さなファイル（256 MB以下）に適用されます。 ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。
>
>- バッチ取り込みの入力として、複数行のJSONの代わりに単行のJSONを使用します。 1行のJSONでは、1つの入力ファイルを複数のチャンクに分割して並行して処理できるため、パフォーマンスが向上します。一方、複数行のJSONは分割できません。 これにより、データ処理コストを大幅に削減し、バッチ処理の遅延を向上させることができます。

### バッチの作成

まず、JSON を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。

>[!NOTE]
>
>以下の例は、1行のJSON用です。 複数行の JSON を取得するには、`isMultiLineJson` フラグを設定する必要があります。詳しくは、[バッチ取り込みトラブルシューティングガイド](./troubleshooting.md)を参照してください。

**API 形式**

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
                "format": "json"
           }
      }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | 参照データセットの ID。 |

**応答**

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{ORG_ID}",
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

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチの ID。 |
| `{DATASET_ID}` | 参照先のデータセットの ID。 |

### ファイルのアップロード

バッチを作成したら、バッチ作成応答のバッチ IDを使用してファイルをバッチにアップロードできます。 複数のファイルをバッチにアップロードできます。

>[!NOTE]
>
>適切にフォーマットされたJSON データファイル [の](#data-transformation-for-batch-ingestion)例については、付録の節を参照してください。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 一意のファイル名を使用して、送信するファイルのバッチで別のファイルと競合しないようにしてください。 |

**リクエスト**

>[!NOTE]
>
>このAPIは、1部アップロードをサポートしています。 content-type が application/octet-stream であることを確認します。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.json \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/octet-stream' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.json"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイル パスは、`acme/customers/campaigns/summer.json`などのローカル ファイル パスです。 |

**応答**

```http
200 OK
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API 形式**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |

**リクエスト**

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```http
200 OK
```

## Parquet ファイルの取得 {#ingest-parquet-files}

>[!NOTE]
>
>次の手順は、小さなファイル（256 MB以下）に適用されます。 ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

### バッチの作成

まず、Parquet を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。

**リクエスト**

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "Content-Type: application/json" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-api-key: {API_KEY}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" 
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
                "format": "parquet"
           }
      }'
```

| パラメーター | 説明 |
| --------- | ------------ |
| `{DATASET_ID}` | 参照データセットの ID。 |

**応答**

```http
201 Created
```

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{ORG_ID}",
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

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチの ID。 |
| `{DATASET_ID}` | 参照先のデータセットの ID。 |
| `{USER_ID}` | バッチを作成したユーザーの ID。 |

### ファイルのアップロード

これでバッチが作成され、前の `batchId` を使用してファイルをバッチにアップロードできます。複数のファイルをバッチにアップロードできます。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 一意のファイル名を使用して、送信するファイルのバッチで別のファイルと競合しないようにしてください。 |

**リクエスト**

>[!CAUTION]
>
>このAPIは、1部アップロードをサポートしています。 content-type が application/octet-stream であることを確認します。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/octet-stream' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイル パスは、`acme/customers/campaigns/summer.parquet`などのローカル ファイル パスです。 |

**応答**

```http
200 OK
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API 形式**

```http
POST /batches/{BATCH_ID}?action=complete
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | シグナルを送信するバッチの ID は、完了の準備が整っています。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
200 OK
```

## 大きな Parquet ファイルの取得

>[!NOTE]
>
>この節では、256 MBを超えるファイルをアップロードする方法について詳しく説明します。 大きなファイルはチャンク単位でアップロードされ、API 信号を介して繋ぎ合わされます。

### バッチの作成

まず、Parquet を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。

**API 形式**

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
             "format": "parquet"
           }
      }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | 参照データセットの ID。 |

**応答**

```http
201 Created
```

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{ORG_ID}",
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

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチの ID。 |
| `{DATASET_ID}` | 参照先のデータセットの ID。 |
| `{USER_ID}` | バッチを作成したユーザーの ID。 |

### 大きいファイルの初期化

バッチを作成した後、大きなファイルを初期化してから、バッチにチャンクをアップロードする必要があります。

**API 形式**

```http
POST /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチの ID。 |
| `{DATASET_ID}` | 参照先のデータセットの ID。 |
| `{FILE_NAME}` | 初期化されるファイルの名前。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet?action=INITIALIZE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
201 Created
```

### 大きなファイルチャンクのアップロード

ファイルが作成されたので、以降のすべてのチャンクは、ファイルの各セクションに対して 1 つずつ、繰り返し PATCH リクエストを実行することでアップロードできます。

**API 形式**

```http
PATCH /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 一意のファイル名を使用して、送信するファイルのバッチで別のファイルと競合しないようにしてください。 |

**リクエスト**

>[!CAUTION]
>
>このAPIは、1部アップロードをサポートしています。 content-type が application/octet-stream であることを確認します。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/octet-stream' \
  -H 'Content-Range: bytes {CONTENT_RANGE}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONTENT_RANGE}` | 指定した範囲の開始と終了を整数で指定します。 |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイル パスは、`acme/customers/campaigns/summer.json`などのローカル ファイル パスです。 |


**応答**

```http
200 OK
```

### 完全な大きいファイル

これでバッチが作成され、前の `batchId` を使用してファイルをバッチにアップロードできます。複数のファイルをバッチにアップロードできます。

**API 形式**

```http
POST /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 完了を伝えるバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | 完了を伝えるファイルの名前。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
201 Created
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API 形式**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | シグナルを送信するバッチの ID が完了しました。 |


**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
200 OK
```

## CSV ファイルの取得

CSV ファイルを取得するには、CSV をサポートするクラス、スキーマ、データセットを作成する必要があります。必要なクラスとスキーマの作成方法について詳しくは、『[アドホックスキーマの作成チュートリアル](../../xdm/api/ad-hoc.md)』の手順に従ってください。

>[!NOTE]
>
>次の手順は、小さなファイル（256 MB以下）に適用されます。 ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

### データセットの作成

上記の手順に従って必要なクラスとスキーマを作成した後、CSV サポートするデータセットを作成する必要があります。

**API 形式**

```http
POST /catalog/dataSets
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "{DATASET_NAME}",
      "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
          "contentType": "application/vnd.adobe.xed+json;version=1"
      }
  }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TENANT_ID}` | このIDは、作成するリソースの名前空間が適切に設定され、組織内に含まれていることを確認するために使用されます。 |
| `{SCHEMA_ID}` | 作成したスキーマの ID。 |

### バッチの作成

次に、CSV を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされたスキーマに適合していることを確認する必要があります。

**API 形式**

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
            "datasetId": "{DATASET_ID}",
            "inputFormat": {
                "format": "csv"
            }
      }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | 参照データセットの ID。 |

**応答**

```http
201 Created
```

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{ORG_ID}",
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

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチの ID。 |
| `{DATASET_ID}` | 参照先のデータセットの ID。 |
| `{USER_ID}` | バッチを作成したユーザーの ID。 |

### ファイルのアップロード

これでバッチが作成され、前の `batchId` を使用してファイルをバッチにアップロードできます。複数のファイルをバッチにアップロードできます。

>[!NOTE]
>
>適切にフォーマットされたCSV データファイル [の](#data-transformation-for-batch-ingestion)例については、付録の節を参照してください。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 一意のファイル名を使用して、送信するファイルのバッチで別のファイルと競合しないようにしてください。 |

**リクエスト**

>[!CAUTION]
>
>このAPIは、1部アップロードをサポートしています。 content-type が application/octet-stream であることを確認します。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.csv \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/octet-stream' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.csv"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイル パスは、`acme/customers/campaigns/summer.csv`などのローカル ファイル パスです。 |


**応答**

```http
200 OK
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API 形式**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```http
200 OK
```

## バッチのキャンセル

バッチの処理中は、キャンセルすることができます。ただし、バッチが確定されると（成功または失敗の状態など）、バッチはキャンセルできません。

**API 形式**

```http
POST /batches/{BATCH_ID}?action=ABORT
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | キャンセルするバッチの ID。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=ABORT \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
200 OK
```

## バッチの削除 {#delete-a-batch}

`action=REVERT` クエリパラメーターを使用して、削除するバッチの ID に対して次の POST リクエストを実行すると、バッチを削除できます。バッチは「非アクティブ」と指定され、ガベージコレクションの対象となります。バッチは非同期で収集され、その時点で「削除済み」と指定されます。

**API 形式**

```http
POST /batches/{BATCH_ID}?action=REVERT
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 削除するバッチの ID。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=REVERT \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
200 OK
```

## バッチのパッチ

組織のプロファイルストアのデータを更新する必要が生じる場合があります。 例えば、レコードを修正したり、属性値を変更したりする必要がある場合があります。Adobe Experience Platformでは、アップサートアクションまたは「バッチのパッチ適用」を通じて、プロファイルストアデータの更新またはパッチをサポートしています。

>[!NOTE]
>
>これらの更新は、エクスペリエンスイベントではなく、プロファイルレコードでのみ許可されます。

バッチにパッチを適用するには、次の手順が必要です。

- **プロファイルと属性の更新に対するデータセットの有効化。**&#x200B;これはデータセット タグを通じて行われます。特定の`isUpsert:true` タグを`unifiedProfile`配列に追加する必要があります。 データセットを作成する方法や、アップサート用に既存のデータセットを設定する方法を示す詳細な手順については、[&#x200B; プロファイル更新のためのデータセットの有効化](../../catalog/datasets/enable-upsert.md)のチュートリアルに従ってください。
- **パッチを適用するフィールドとプロファイルのID フィールドを含むParquet ファイル。** バッチにパッチを適用するためのデータ形式は、通常のバッチ取り込みプロセスと似ています。 必要な入力はParquet ファイルで、更新するフィールドに加えて、プロファイルストアのデータと一致させるには、アップロードされたデータにID フィールドが含まれている必要があります。

プロファイルとアップサートに対してデータセットを有効にし、パッチを適用するフィールドと必要なID フィールドを含むParquet ファイルを作成したら、[Parquet ファイルの取り込み](#ingest-parquet-files)の手順に従って、バッチ取り込みを介してパッチを完了できます。

## バッチの再生

既に取得したバッチを置き換える場合は、「バッチ再生」を使用できます。このアクションは、古いバッチを削除し、代わりに新しいバッチを取得するアクションと同じです。

### バッチの作成

まず、JSON を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。また、再生セクションで参照として古いバッチを指定する必要があります。次の例では、ID `batchIdA`と`batchIdB`を持つバッチを再生しています。

**API 形式**

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
             "format": "json"
           },
            "replay": {
                "predecessors": ["${batchIdA}","${batchIdB}"],
                "reason": "replace"
             }
      }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | 参照データセットの ID。 |

**応答**

```http
201 Created
```

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{ORG_ID}",
    "updated": 0,
    "status": "loading",
    "created": 0,
    "relatedObjects": [
        {
            "type": "dataSet",
            "id": "{DATASET_ID}"
        }
    ],
    "replay": {
        "predecessors": [
            "batchIdA", "batchIdB"
        ],
        "reason": "replace"
    },
    "version": "1.0.0",
    "tags": {},
    "createdUser": "{USER_ID}",
    "updatedUser": "{USER_ID}"
}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチの ID。 |
| `{DATASET_ID}` | 参照先のデータセットの ID。 |
| `{USER_ID}` | バッチを作成したユーザーの ID。 |


### ファイルのアップロード

これでバッチが作成され、前の `batchId` を使用してファイルをバッチにアップロードできます。複数のファイルをバッチにアップロードできます。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 一意のファイル名を使用して、送信するファイルのバッチで別のファイルと競合しないようにしてください。 |

**リクエスト**

>[!CAUTION]
>
>このAPIは、1部アップロードをサポートしています。 content-type が application/octet-stream であることを確認します。API と互換性のないマルチパートリクエストがデフォルトで設定されるので、curl -F オプションは使用しないでください。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.json \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/octet-stream' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.json"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイル パスは、`acme/customers/campaigns/summer.json`などのローカル ファイル パスです。 |

**応答**

```http
200 OK
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API 形式**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 完了するバッチの ID。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```http
200 OK
```

## 付録

次の節では、バッチ取り込みを使用してExperience Platformにデータを取り込む方法について説明します。

### バッチ取り込み用のデータ変換

データファイルを[!DNL Experience Platform]に取り込むには、ファイルの階層構造が、アップロード先のデータセットに関連付けられた[Experience Data Model （XDM） &#x200B;](../../xdm/home.md) スキーマに準拠している必要があります。

XDM スキーマに準拠する CSV ファイルのマッピング方法に関する情報は、[サンプル変換](../../etl/transformations.md)ドキュメントに記載されている情報と、適切に書式設定された JSON データファイルの例を参照してください。このドキュメントのサンプルファイルは、次の場所にあります。

- [CRM_profiles.csv](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.csv)
- [CRM_profiles.json](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.json)
