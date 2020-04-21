---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Batch Ingestion開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 6c17351b04fedefd4b57b9530f1d957da8183a68

---


# バッチインジェスト開発者ガイド

このドキュメントでは、バッチ取り込みAPIの使用に関する包括的 [な概要を説明しま](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)す。

このドキュメントの付録では、サンプルCSV [やJSONデータファイルなど](#data-transformation-for-batch-ingestion)、取り込みに使用するデータの形式設定に関する情報を提供します。

## はじめに

データ取り込みでは、RESTful APIを使用して、サポートされるオブジェクトタイプに対して基本的なCRUD操作を実行できます。

次の節では、バッチインジェストAPIを正しく呼び出すために知っておく必要がある、または手元に置く必要がある追加情報を示します。

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

- [バッチインジェスト](./overview.md):データをバッチファイルとしてAdobe Experience Platformに取り込むことができます。
- [Experience Data Model(XDM)System](../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
- [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

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

ペイロード(POST、PUT、PATCH)を含むリクエストには、追加のヘッダーが必要な場合があ `Content-Type` ります。 各呼び出しに固有の受け入れられた値は、呼び出しパラメーターで提供されます。 このガイドでは、次のコンテンツタイプを使用します。

- コンテンツタイプ：application/json
- コンテンツタイプ：application/octet-stream

## タイプ

データを取り込む場合は、Experience Data Model(XDM)スキーマの動作を理解することが重要です。 XDMのフィールドタイプを様々な形式にマップする方法について詳しくは、『 [スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md)』を参照。

データを取り込む際には、柔軟性があります。ターゲットスキーマ内のデータとタイプが一致しない場合、データは表現されたターゲットタイプに変換されます。 できない場合は、バッチが `TypeCompatibilityException`.

例えば、JSONもCSVも日付や時刻のタイプを持ちません。 その結果、これらの値は [ISO 8061形式の文字列](https://www.iso.org/iso-8601-date-and-time-format.html) (&quot;2018-07-10T15:05:59.000-08:00&quot;)またはUnix時間形式のミリ秒(153126395)を使用して表されます。9000)に変換され、取り込み時にターゲットXDMタイプに変換されます。

次の表に、データの取り込み時にサポートされる変換を示します。

| 受信（行）とターゲット（列） | 文字列 | バイト | 短い | 整数 | ロング | 重複 | 日付 | 日付 — 時刻 | オブジェクト | マップ |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 文字列 | X | X | X | X | X | X | X | X |  |  |
| バイト | X | X | X | X | X | X |  |  |  |  |
| 短い | X | X | X | X | X | X |  |  |  |  |
| 整数 | X | X | X | X | X | X |  |  |  |  |
| ロング | X | X | X | X | X | X | X | X |  |  |
| 重複 | X | X | X | X | X | X |  |  |  |  |
| 日付 |  |  |  |  |  |  | X |  |  |  |
| 日付 — 時刻 |  |  |  |  |  |  |  | X |  |  |
| オブジェクト |  |  |  |  |  |  |  |  | X | X |
| マップ |  |  |  |  |  |  |  |  | X | X |

>[!NOTE] ブール値と配列は他の型に変換できません。

## 取り込みの制約

バッチデータの取り込みには、いくつかの制約があります。
- バッチあたりの最大ファイル数：1500
- 最大バッチサイズ：100 GB
- 1行あたりのプロパティまたはフィールドの最大数：10000
- 1ユーザーあたりの1分あたりの最大バッチ数：138

## JSONファイルの取り込み

>[!NOTE] 次の手順は、小さいファイル（256 MB以下）に適用されます。 ゲートウェイのタイムアウトまたは要求本文のサイズのエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

### バッチの作成

まず、JSONを入力形式としてバッチを作成する必要があります。 バッチを作成する場合は、データセットIDを指定する必要があります。 また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされたXDMスキーマに適合していることを確認する必要があります。

>[!NOTE] 以下に、1行のJSONの例を示します。 複数行のJSONを取り込むには、フラグ `isMultiLineJson` を設定する必要があります。 詳しくは、バッチインジェストのトラブルシューティ [ングガイドを参照してくださ](./troubleshooting.md)い。

**API形式**

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
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
| `{DATASET_ID}` | 参照データセットのID。 |

**応答**

```json
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

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチのID。 |
| `{DATASET_ID}` | 参照先のデータセットのID。 |

### ファイルのアップロード

これでバッチが作成され、前のからを使用してフ `batchId` ァイルをバッチにアップロードできます。 複数のファイルをバッチにアップロードできます。

>[!NOTE] 適切にフォーマットされたJSONデ [ータファイルの例については、付録の節を参照してください](#data-transformation-for-batch-ingestion)。

**API形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチのID。 |
| `{DATASET_ID}` | バッチの参照データセットのID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 |

**リクエスト**

>[!NOTE] APIは、シングルパートのアップロードをサポートします。 コンテンツタイプがapplication/octet-streamであることを確認します。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.json \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/octet-stream' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.json"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 |

**応答**

```http
200 OK
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API形式**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチのID。 |

**リクエスト**

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```http
200 OK
```

## インジェストパーケファイル

>[!NOTE] 次の手順は、小さいファイル（256 MB以下）に適用されます。 ゲートウェイのタイムアウトまたは要求本文のサイズのエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

### バッチの作成

まず、Parquetを入力形式としてバッチを作成する必要があります。 バッチを作成する場合は、データセットIDを指定する必要があります。 また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされたXDMスキーマに適合していることを確認する必要があります。

**リクエスト**

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "Content-Type: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-api-key : {API_KEY}" \
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
| `{DATASET_ID}` | 参照データセットのID。 |

**応答**

```http
201 Created
```

```json
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

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチのID。 |
| `{DATASET_ID}` | 参照先のデータセットのID。 |
| `{USER_ID}` | バッチを作成したユーザーのID。 |

### ファイルのアップロード

これでバッチが作成され、前のからを使用してフ `batchId` ァイルをバッチにアップロードできます。 複数のファイルをバッチにアップロードできます。

**API形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチのID。 |
| `{DATASET_ID}` | バッチの参照データセットのID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 |

**リクエスト**

>[!CAUTION] このAPIは、シングルパートのアップロードをサポートします。 コンテンツタイプがapplication/octet-streamであることを確認します。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/octet-stream' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 |

**応答**

```http
200 OK
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API形式**

```http
POST /batches/{BATCH_ID}?action=complete
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | シグナルを送信するバッチのIDは、完了の準備が整っています。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
200 OK
```

## 大きなパーケファイルを取り込む

>[!NOTE] この節では、256 MBを超えるファイルをアップロードする方法について説明します。 大きなファイルはチャンク単位でアップロードされ、API信号を介して繋ぎ合わされます。

### バッチの作成

まず、Parquetを入力形式としてバッチを作成する必要があります。 バッチを作成する場合は、データセットIDを指定する必要があります。 また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされたXDMスキーマに適合していることを確認する必要があります。

**API形式**

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
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
| `{DATASET_ID}` | 参照データセットのID。 |

**応答**

```http
201 Created
```

```json
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

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチのID。 |
| `{DATASET_ID}` | 参照先のデータセットのID。 |
| `{USER_ID}` | バッチを作成したユーザーのID。 |

### 大きいファイルの初期化

バッチを作成した後、大きなファイルを初期化してから、バッチにチャンクをアップロードする必要があります。

**API形式**

```http
POST /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチのID。 |
| `{DATASET_ID}` | 参照先のデータセットのID。 |
| `{FILE_NAME}` | 初期化されるファイルの名前。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet?action=INITIALIZE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
201 Created
```

### 大きなファイルチャンクのアップロード

ファイルが作成されたので、以降のすべてのチャンクは、ファイルの各セクションに対して1つずつ、繰り返しPATCHリクエストを行うことでアップロードできます。

**API形式**

```http
PATCH /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチのID。 |
| `{DATASET_ID}` | バッチの参照データセットのID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 |

**リクエスト**

>[!CAUTION] このAPIは、シングルパートのアップロードをサポートします。 コンテンツタイプがapplication/octet-streamであることを確認します。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/octet-stream' \
  -H 'Content-Range: bytes {CONTENT_RANGE}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONTENT_RANGE}` | 整数で指定します。指定した範囲の開始と終了を指定します。 |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 |


**応答**

```http
200 OK
```

### 完全な大きいファイル

これでバッチが作成され、前のからを使用してフ `batchId` ァイルをバッチにアップロードできます。 複数のファイルをバッチにアップロードできます。

**API形式**

```http
POST /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 完了を伝えるバッチのID。 |
| `{DATASET_ID}` | バッチの参照データセットのID。 |
| `{FILE_NAME}` | 完了を伝えるファイルの名前。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.parquet?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
201 Created
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API形式**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | シグナルを送信するバッチのIDが完了した。 |


**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
200 OK
```

## CSVファイルの取り込み

CSVファイルを取り込むには、CSVをサポートするクラス、スキーマ、データセットを作成する必要があります。 必要なクラスとスキーマの作成方法について詳しくは、「アドホックスキーマの作成」チュートリ [アルの手順に従ってください](../../xdm/api/ad-hoc.md)。

>[!NOTE] 次の手順は、小さいファイル（256 MB以下）に適用されます。 ゲートウェイのタイムアウトまたは要求本文のサイズのエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

### データセットの作成

上記の手順に従って必要なクラスとスキーマを作成した後、CSVをサポートするデータセットを作成する必要があります。

**API形式**

```http
POST /catalog/dataSets
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "{DATASET_NAME}",
      "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
          "contentType": "application/vnd.adobe.xed+json;version=1"
      },
      "fileDescription": {
          "format": "parquet",
          "delimiters": [","], 
          "quotes": ["\""],
          "escapes": ["\\"],
          "header": true,
          "charset": "UTF-8"
      }      
  }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TENANT_ID}` | このIDは、作成したリソースの名前が適切に付けられ、IMS組織内に含まれていることを確認するために使用されます。 |
| `{SCHEMA_ID}` | 作成したスキーマのID。 |

JSON本文の「fileDescription」セクションの異なる部分の説明を以下に示します。

```json
{
    "fileDescription": {
        "format": "parquet",
        "delimiters": [","],
        "quotes": ["\""],
        "escapes": ["\\"],
        "header": true,
        "charset": "UTF-8"
    }
}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `format` | 入力ファイルの形式ではなく、マスターファイルの形式です。 |
| `delimiters` | 区切り文字として使用する文字です。 |
| `quotes` | 引用符に使用する文字。 |
| `escapes` | エスケープ文字として使用する文字です。 |
| `header` | アップロードするファイルには **ヘッダー** が含まれている必要があります。 スキーマの検証が行われるので、この値をtrueに設定する必要があります。 また、ヘッダーにスペースを含め **ることは** できません。ヘッダーにスペースが含まれている場合は、アンダースコアで置き換えてください。 |
| `charset` | オプションのフィールドです。 その他のサポートされている文字セットには、「US-ASCII」と「ISO-8869-1」があります。 空のままにすると、デフォルトでUTF-8が使用されます。 |

参照するデータセットには、上記のファイル記述ブロックが含まれ、レジストリ内の有効なスキーマを指す必要があります。 そうしないと、ファイルはパーケーにマスターされません。

### バッチの作成

次に、CSVを入力形式としてバッチを作成する必要があります。 バッチを作成する場合は、データセットIDを指定する必要があります。 また、バッチの一部としてアップロードされたすべてのファイルが、提供されたデータセットにリンクされたスキーマに適合していることを確認する必要があります。

**API形式**

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
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
| `{DATASET_ID}` | 参照データセットのID。 |

**応答**

```http
201 Created
```

```json
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

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 新しく作成されたバッチのID。 |
| `{DATASET_ID}` | 参照先のデータセットのID。 |
| `{USER_ID}` | バッチを作成したユーザーのID。 |

### ファイルのアップロード

これでバッチが作成され、前のからを使用してフ `batchId` ァイルをバッチにアップロードできます。 複数のファイルをバッチにアップロードできます。

>[!NOTE] 適切にフォーマットされたCSVデ [ータファイルの例については、付録の節を参照してくださ](#data-transformation-for-batch-ingestion)い。

**API形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチのID。 |
| `{DATASET_ID}` | バッチの参照データセットのID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 |

**リクエスト**

>[!CAUTION] このAPIは、シングルパートのアップロードをサポートします。 コンテンツタイプがapplication/octet-streamであることを確認します。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.csv \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/octet-stream' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.csv"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 |


**応答**

```http
200 OK
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API形式**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```http
200 OK
```

## バッチのキャンセル

バッチの処理中は、キャンセルすることができます。 ただし、バッチが確定されると（成功または失敗の状態など）、バッチはキャンセルできません。

**API形式**

```http
POST /batches/{BATCH_ID}?action=ABORT
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | キャンセルするバッチのID。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=ABORT \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
200 OK
```

## バッチの削除 {#delete-a-batch}

削除するバッチのIDに対する `action=REVERT` クエリパラメーターを使用して、次のPOSTリクエストを実行すると、バッチを削除できます。 バッチは「非アクティブ」とマークされ、ガベージコレクションの対象となります。 バッチは非同期で収集され、その時点で「削除済み」とマークされます。

**API形式**

```http
POST /batches/{BATCH_ID}?action=REVERT
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 削除するバッチのID。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=REVERT \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

```http
200 OK
```

## バッチの再生

既に取り込んだバッチを置き換える場合は、「バッチ再生」を使用して置き換えることができます。この操作は、古いバッチを削除し、代わりに新しいバッチを取り込む操作と同じです。

### バッチの作成

まず、JSONを入力形式としてバッチを作成する必要があります。 バッチを作成する場合は、データセットIDを指定する必要があります。 また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされたXDMスキーマに適合していることを確認する必要があります。 また、再生セクションで参照として古いバッチを指定する必要があります。 次の例では、IDとを使用してバッチを再生し `batchIdA` ます `batchIdB`。

**API形式**

```http
POST /batches
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
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
| `{DATASET_ID}` | 参照データセットのID。 |

**応答**

```http
201 Created
```

```json
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
| `{BATCH_ID}` | 新しく作成されたバッチのID。 |
| `{DATASET_ID}` | 参照先のデータセットのID。 |
| `{USER_ID}` | バッチを作成したユーザーのID。 |


### ファイルのアップロード

これでバッチが作成され、前のからを使用してフ `batchId` ァイルをバッチにアップロードできます。 複数のファイルをバッチにアップロードできます。

**API形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチのID。 |
| `{DATASET_ID}` | バッチの参照データセットのID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 |

**リクエスト**

>[!CAUTION] このAPIは、シングルパートのアップロードをサポートします。 コンテンツタイプがapplication/octet-streamであることを確認します。 APIと互換性のないマルチパートリクエストがデフォルトで設定されるので、curl -Fオプションは使用しないでください。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.json \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/octet-stream' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  --data-binary "@{FILE_PATH_AND_NAME}.json"
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。 |

**応答**

```http
200 OK
```

### バッチの完了

ファイルの様々な部分のアップロードが完了したら、データが完全にアップロードされ、バッチがプロモーションの準備ができたことを伝える必要があります。

**API形式**

```http
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 完了するバッチのID。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```http
200 OK
```

## 付録

### バッチ取り込み用のデータ変換

データファイルをExperience Platformに取り込むには、ファイルの階層構造が、アップロード先のデータセットに関連付けられた [Experience Data Model(XDM)](../../xdm/home.md) スキーマに準拠している必要があります。

XDMスキーマに準拠するCSVファイルのマッピング方法に関する情報は、サンプル変換 [](../../etl/transformations.md) ドキュメントに記載されている情報と、適切にフォーマットされたJSONデータファイルの例を参照してください。 このドキュメントのサンプルファイルは、次の場所にあります。

- [CRM_プロファイル.csv](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.csv)
- [CRM_プロファイル.json](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.json)