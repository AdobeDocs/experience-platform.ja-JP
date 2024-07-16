---
keywords: Experience Platform；ホーム；人気のトピック；バッチ取得；バッチ取得；取得；developer guide;api guide；アップロード；Parquet の取り込み；json の取り込み；
solution: Experience Platform
title: バッチ取り込み API ガイド
description: このドキュメントでは、Adobe Experience Platformのバッチ取得 API を使用する開発者向けの包括的なガイドを提供します。
exl-id: 4ca9d18d-1b65-4aa7-b608-1624bca19097
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '2383'
ht-degree: 65%

---

# バッチ取得開発者ガイド

このドキュメントでは、Adobe Experience Platformで [ バッチ取得 API エンドポイント ](https://developer.adobe.com/experience-platform-apis/references/batch-ingestion/) を使用する際の包括的なガイドを提供します。 前提条件やベストプラクティスを含む、バッチ取得 API の概要については、まず [ バッチ取得 API の概要 ](overview.md) をお読みください。

このドキュメントの付録では、CSV 例や JSON データファイル例など、[取得に使用するデータの形式設定](#data-transformation-for-batch-ingestion)に関する情報を提供します。

## はじめに

このガイドで使用する API エンドポイントは、[ バッチ取得 API](https://developer.adobe.com/experience-platform-apis/references/batch-ingestion/) の一部です。 バッチ取得は、サポートされているオブジェクトタイプに対して基本的な CRUD 操作を実行できる RESTful API を通じて提供されます。

続行する前に、[ バッチ取得 API の概要 ](overview.md) および [ はじめる前に ](getting-started.md) を確認してください。

## JSON ファイルの取得

>[!NOTE]
>
>次の手順は、小さいファイル（256 MB 以下）に適用されます。 ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

### バッチの作成

まず、JSON を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。

>[!NOTE]
>
>以下の例は、1 行の JSON に対するものです。 複数行の JSON を取得するには、`isMultiLineJson` フラグを設定する必要があります。詳しくは、『[バッチ取得トラブルシューティングガイド](./troubleshooting.md)』を参照してください。

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

バッチを作成したので、バッチ作成応答のバッチ ID を使用してファイルをバッチにアップロードできます。 複数のファイルをバッチにアップロードできます。

>[!NOTE]
>
>[ 正しい形式の JSON データファイルの例 ](#data-transformation-for-batch-ingestion) については、付録の節を参照してください。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 送信するファイルのバッチに対して別のファイルと競合しないように、一意のファイル名を使用してください。 |

**リクエスト**

>[!NOTE]
>
>この API は、シングルパートのアップロードをサポートします。 content-type が application/octet-stream であることを確認します。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイルパスは、`acme/customers/campaigns/summer.json` などのローカルファイルパスです。 |

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
>次の手順は、小さいファイル（256 MB 以下）に適用されます。 ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

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
| `{FILE_NAME}` | アップロードするファイルの名前。 送信するファイルのバッチに対して別のファイルと競合しないように、一意のファイル名を使用してください。 |

**リクエスト**

>[!CAUTION]
>
>この API は、シングルパートのアップロードをサポートします。 content-type が application/octet-stream であることを確認します。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイルパスは、`acme/customers/campaigns/summer.parquet` などのローカルファイルパスです。 |

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
>この節では、256 MB を超えるファイルのアップロード方法について詳しく説明します。 大きなファイルはチャンク単位でアップロードされ、API 信号を介して繋ぎ合わされます。

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
| `{FILE_NAME}` | アップロードするファイルの名前。 送信するファイルのバッチに対して別のファイルと競合しないように、一意のファイル名を使用してください。 |

**リクエスト**

>[!CAUTION]
>
>この API は、シングルパートのアップロードをサポートします。 content-type が application/octet-stream であることを確認します。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイルパスは、`acme/customers/campaigns/summer.json` などのローカルファイルパスです。 |


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
>次の手順は、小さいファイル（256 MB 以下）に適用されます。 ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

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
| `{TENANT_ID}` | この ID は、作成したリソースの名前空間が適切に設定され、組織内に格納されていることを確認するために使用されます。 |
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
>[ 適切な形式の CSV データファイルの例 ](#data-transformation-for-batch-ingestion) については、付録の節を参照してください。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 送信するファイルのバッチに対して別のファイルと競合しないように、一意のファイル名を使用してください。 |

**リクエスト**

>[!CAUTION]
>
>この API は、シングルパートのアップロードをサポートします。 content-type が application/octet-stream であることを確認します。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイルパスは、`acme/customers/campaigns/summer.csv` などのローカルファイルパスです。 |


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

## バッチのパッチ適用

場合によっては、組織のプロファイルストアのデータを更新する必要があります。 例えば、レコードを修正したり、属性値を変更したりする必要がある場合があります。Adobe Experience Platformは、アップサートアクションまたは「バッチのパッチ適用」を通じたプロファイルストアデータの更新またはパッチ適用をサポートします。

>[!NOTE]
>
>これらの更新は、プロファイルレコードでのみ許可され、エクスペリエンスイベントでは許可されません。

バッチにパッチを適用するには、次の操作が必要です。

- **プロファイルおよび属性の更新が有効になっているデータセット。** これはデータセットタグを使用しておこなわれ、特定の `isUpsert:true` タグを `unifiedProfile` 配列に追加する必要があります。 データセットの作成またはアップサート用の既存のデータセットの設定の手順について詳しくは、[ プロファイル更新のためのデータセットの有効化 ](../../catalog/datasets/enable-upsert.md) に関するチュートリアルに従ってください。
- **パッチを適用するフィールドと、プロファイルの ID フィールドを含む Parquet ファイル。** バッチにパッチを適用するためのデータ形式は、通常のバッチ取得プロセスと似ています。 必要な入力は Parquet ファイルであり、更新するフィールドに加えて、プロファイルストアのデータと一致させるために、アップロードされたデータに ID フィールドが含まれている必要があります。

プロファイルとアップサートが有効なデータセットと、パッチを適用するフィールドおよび必要な ID フィールドを含む Parquet ファイルが完成したら、[Parquet ファイルの取り込み ](#ingest-parquet-files) の手順に従って、バッチ取り込みを使用してパッチを完了できます。

## バッチの再生

既に取得したバッチを置き換える場合は、「バッチ再生」を使用できます。このアクションは、古いバッチを削除し、代わりに新しいバッチを取得するアクションと同じです。

### バッチの作成

まず、JSON を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。また、再生セクションで参照として古いバッチを指定する必要があります。次の例では、ID `batchIdA` および `batchIdB` を持つバッチを再生しています。

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
| `{FILE_NAME}` | アップロードするファイルの名前。 送信するファイルのバッチに対して別のファイルと競合しないように、一意のファイル名を使用してください。 |

**リクエスト**

>[!CAUTION]
>
>この API は、シングルパートのアップロードをサポートします。 content-type が application/octet-stream であることを確認します。API と互換性のないマルチパートリクエストがデフォルトで設定されるので、curl -F オプションは使用しないでください。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 このファイルパスは、`acme/customers/campaigns/summer.json` などのローカルファイルパスです。 |

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

次の節では、バッチ取得を使用してExperience Platformでデータを取り込む方法について説明します。

### バッチ取得用のデータ変換

データファイルを [!DNL Experience Platform] に取り込むには、ファイルの階層構造が、アップロード先のデータセットに関連付けられている [ エクスペリエンスデータモデル（XDM） ](../../xdm/home.md) スキーマに準拠している必要があります。

XDM スキーマに準拠する CSV ファイルのマッピング方法に関する情報は、[サンプル変換](../../etl/transformations.md)ドキュメントに記載されている情報と、適切に書式設定された JSON データファイルの例を参照してください。このドキュメントのサンプルファイルは、次の場所にあります。

- [CRM_profiles.csv](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.csv)
- [CRM_profiles.json](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.json)
