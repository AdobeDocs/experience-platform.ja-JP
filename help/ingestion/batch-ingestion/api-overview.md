---
keywords: Experience Platform;home;popular topics;batch ingestion;Batch ingestion;ingestion;developer guide;api guide;upload;ingest parquet;ingest json;
solution: Experience Platform
title: Batch Ingestion開発者ガイド
topic: developer guide
description: このドキュメントでは、バッチ取得 API の使用に関する包括的な概要を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '2675'
ht-degree: 89%

---


# バッチ取得開発者ガイド

このドキュメントでは、[バッチ取得 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) の使用に関する包括的な概要を説明します。

このドキュメントの付録では、CSV 例や JSON データファイル例など、[取得に使用するデータの形式設定](#data-transformation-for-batch-ingestion)に関する情報を提供します。

## はじめに

データ取得では、RESTful API を使用して、サポートされるオブジェクトタイプに対して基本的な CRUD 操作を実行できます。

次の節では、バッチ取得 API を正しく呼び出すために知っておく必要がある、または手元に置く必要がある追加情報を示します。

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

- [バッチ取得](./overview.md)：データをバッチファイルとして Adobe Experience Platform に取得することができます。
- [[!DNL Experience Data Model (XDM)] システム](../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
- [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むリクエストには、追加の `Content-Type` ヘッダーが必要な場合があります。各呼び出しに固有の受け入れられた値は、呼び出しパラメーターで提供されます。

## タイプ

When ingesting data, it is important to understand how [!DNL Experience Data Model] (XDM) schemas work. XDM のフィールドタイプを様々な形式にマップする方法について詳しくは、『[スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md)』を参照してください。

データ取得には柔軟性があります。ターゲットスキーマ内のデータとタイプが一致しない場合、データは表現されたターゲットタイプに変換されます。  できない場合は、バッチが `TypeCompatibilityException` で失敗します。

例えば、JSON も CSV も日付や時刻のタイプを持ちません。その結果、これらの値は [ISO 8061 形式の文字列](https://www.iso.org/iso-8601-date-and-time-format.html)（「2018-07-10T15:05:59.000-08:00」）または Unix 時間形式のミリ秒（1531263959000）を使用して表され、取得時にターゲット XDM タイプに変換されます。

次の表に、データの取得時にサポートされる変換を示します。

| 受信（行）とターゲット（列） | String | Byte | Short | Integer | Long | Double | Date | Date-Time | Object | Map |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| String | X | X | X | X | X | X | X | X |  |  |
| Byte | X | X | X | X | X | X |  |  |  |  |
| Short | X | X | X | X | X | X |  |  |  |  |
| Integer | X | X | X | X | X | X |  |  |  |  |
| Long | X | X | X | X | X | X | X | X |  |  |
| Double | X | X | X | X | X | X |  |  |  |  |
| Date |  |  |  |  |  |  | X |  |  |  |
| Date-Time |  |  |  |  |  |  |  | X |  |  |
| Object |  |  |  |  |  |  |  |  | X | X |
| Map |  |  |  |  |  |  |  |  | X | X |

>[!NOTE]
>
> ブール値と配列は他の型に変換できません。

## 取得の制約

バッチデータ取得には、いくつかの制約があります。
- バッチあたりの最大ファイル数：1500
- 最大バッチサイズ：100GB
- 1 行あたりのプロパティまたはフィールドの最大数：10000
- 1 ユーザーあたりの 1 分あたりの最大バッチ数：138

## JSON ファイルの取得

>[!NOTE]
>
> 次の手順は、小さいファイル（256MB 以下）に適用されます。ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

### バッチの作成

まず、JSON を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。

>[!NOTE]
>
> 以下に、1 行の JSON の例を示します。複数行の JSON を取得するには、`isMultiLineJson` フラグを設定する必要があります。詳しくは、『[バッチ取得トラブルシューティングガイド](./troubleshooting.md)』を参照してください。

**API 形式**

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
| `{DATASET_ID}` | 参照データセットの ID。 |

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
| `{BATCH_ID}` | 新しく作成されたバッチの ID。 |
| `{DATASET_ID}` | 参照先のデータセットの ID。 |

### ファイルのアップロード

これでバッチが作成され、前の `batchId` を使用してファイルをバッチにアップロードできます。複数のファイルをバッチにアップロードできます。

>[!NOTE]
>
>付録の[適切に書式設定された JSON データファイルの例](#data-transformation-for-batch-ingestion)についての節を参照してください。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。このファイルパスは、Adobe側でファイルが保存される場所です。 |

**リクエスト**

>[!NOTE]
>
> API は、シングルパートのアップロードをサポートします。content-type が application/octet-stream であることを確認します。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。このファイルパスは、などのローカルファイルパスで `Users/sample-user/Downloads/sample.json`す。 |

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```http
200 OK
```

## Parquet ファイルの取得

>[!NOTE]
>
> 次の手順は、小さいファイル（256MB 以下）に適用されます。ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

### バッチの作成

まず、Parquet を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。

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
| `{DATASET_ID}` | 参照データセットの ID。 |

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
| `{FILE_NAME}` | アップロードするファイルの名前。このファイルパスは、Adobe側でファイルが保存される場所です。 |

**リクエスト**

>[!CAUTION]
>
> この API は、シングルパートのアップロードをサポートします。content-type が application/octet-stream であることを確認します。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。このファイルパスは、などのローカルファイルパスで `Users/sample-user/Downloads/sample.json`す。 |

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答** 

```http
200 OK
```

## 大きな Parquet ファイルの取得

>[!NOTE]
>
> この節では、256MB を超えるファイルをアップロードする方法について説明します。大きなファイルはチャンク単位でアップロードされ、API 信号を介して繋ぎ合わされます。

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
| `{DATASET_ID}` | 参照データセットの ID。 |

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `{FILE_NAME}` | アップロードするファイルの名前。このファイルパスは、Adobe側でファイルが保存される場所です。 |

**リクエスト**

>[!CAUTION]
>
> この API は、シングルパートのアップロードをサポートします。content-type が application/octet-stream であることを確認します。

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
| `{CONTENT_RANGE}` | 指定した範囲の開始と終了を整数で指定します。 |
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。このファイルパスは、などのローカルファイルパスで `Users/sample-user/Downloads/sample.json`す。 |


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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
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
> 次の手順は、小さいファイル（256MB 以下）に適用されます。ゲートウェイのタイムアウトまたは陸エスト本文のサイズエラーが発生した場合は、大きなファイルのアップロードに切り替える必要があります。

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
| `{TENANT_ID}` | この ID を使用すると、作成したリソースの名前空間が適切に付けられ、IMS 組織内に含まれるようになります |
| `{SCHEMA_ID}` | 作成したスキーマの ID。 |

JSON 本文の「fileDescription」セクションの異なる部分の説明を以下に示します。

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
| `delimiters` | 区切り文字として使用する文字。 |
| `quotes` | 引用符に使用する文字。 |
| `escapes` | エスケープ文字として使用する文字。 |
| `header` | アップロードするファイルにはヘッダーが含まれている&#x200B;**必要**&#x200B;があります。スキーマの検証が行われるので、この値を true に設定する必要があります。また、ヘッダーにスペースを含めることは&#x200B;**できません**。ヘッダーにスペースが含まれている場合は、アンダースコアで置き換えてください。 |
| `charset` | オプションのフィールド。その他のサポートされている文字セットには、「US-ASCII」と「ISO-8869-1」があります。空のままにすると、デフォルトで UTF-8 が使用されます。 |

参照するデータセットには、上記のファイル記述ブロックが含まれ、レジストリ内の有効なスキーマを指す必要があります。そうしないと、ファイルは parquet にマスターされません。

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
| `{DATASET_ID}` | 参照データセットの ID。 |

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
| `{BATCH_ID}` | 新しく作成されたバッチの ID。 |
| `{DATASET_ID}` | 参照先のデータセットの ID。 |
| `{USER_ID}` | バッチを作成したユーザーの ID。 |

### ファイルのアップロード

これでバッチが作成され、前の `batchId` を使用してファイルをバッチにアップロードできます。複数のファイルをバッチにアップロードできます。

>[!NOTE]
>
>付録の[適切に書式設定された CSV データファイルの例](#data-transformation-for-batch-ingestion)についての節を参照してください。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | アップロード先のバッチの ID。 |
| `{DATASET_ID}` | バッチの参照データセットの ID。 |
| `{FILE_NAME}` | アップロードするファイルの名前。このファイルパスは、Adobe側でファイルが保存される場所です。 |

**リクエスト**

>[!CAUTION]
>
> この API は、シングルパートのアップロードをサポートします。content-type が application/octet-stream であることを確認します。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。このファイルパスは、などのローカルファイルパスで `Users/sample-user/Downloads/sample.json`す。 |


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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答** 

```http
200 OK
```

## バッチの再生

既に取得したバッチを置き換える場合は、「バッチ再生」を使用できます。この操作は、古いバッチを削除し、代わりに新しいバッチを取得する操作と同じです。

### バッチの作成

まず、JSON を入力形式としてバッチを作成する必要があります。バッチを作成する場合は、データセット ID を指定する必要があります。また、バッチの一部としてアップロードされるすべてのファイルが、提供されたデータセットにリンクされた XDM スキーマに適合していることを確認する必要があります。また、再生セクションで参照として古いバッチを指定する必要があります。次の例では、`batchIdA` ID と `batchIdB` ID を使用してバッチを再生します 。

**API 形式**

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
| `{DATASET_ID}` | 参照データセットの ID。 |

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
| `{FILE_NAME}` | アップロードするファイルの名前。このファイルパスは、Adobe側でファイルが保存される場所です。 |

**リクエスト**

>[!CAUTION]
>
> この API は、シングルパートのアップロードをサポートします。content-type が application/octet-stream であることを確認します。API と互換性のないマルチパートリクエストがデフォルトで設定されるので、curl -F オプションは使用しないでください。

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
| `{FILE_PATH_AND_NAME}` | アップロードしようとしているファイルのフルパスと名前。このファイルパスは、などのローカルファイルパスで `Users/sample-user/Downloads/sample.json`す。 |

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```http
200 OK
```

## 付録

### バッチ取得用のデータ変換

In order to ingest a data file into [!DNL Experience Platform], the hierarchical structure of the file must comply with the [Experience Data Model (XDM)](../../xdm/home.md) schema associated with the dataset being uploaded to.

XDM スキーマに準拠する CSV ファイルのマッピング方法に関する情報は、[サンプル変換](../../etl/transformations.md)ドキュメントに記載されている情報と、適切に書式設定された JSON データファイルの例を参照してください。このドキュメントのサンプルファイルは、次の場所にあります。

- [CRM_profiles.csv](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.csv)
- [CRM_profiles.json](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.json)