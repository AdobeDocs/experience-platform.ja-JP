---
keywords: Experience Platform；ホーム；人気のあるトピック；バッチ取り込み；バッチ取り込み；部分取り込み；部分取り込みエラー；部分取り込みエラー；部分的なバッチ取り込み；取り込み；取り込み；エラー診断；エラー診断の取り込み；エラーの取り込み；
solution: Experience Platform
title: データ取り込みエラー診断の取得
topic-legacy: overview
description: このドキュメントでは、バッチ取り込みの監視、部分的なバッチ取り込みエラーの管理、および部分的なバッチ取り込みタイプの参照に関する情報を提供します。
exl-id: b885fb00-b66d-453b-80b7-8821117c2041
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '936'
ht-degree: 35%

---

# データ取り込みエラー診断を取り出しています

Adobe Experience Platform でのデータのアップロードと取得には 2 つの方法があります。バッチインジェストを使用すると、様々なファイルタイプ（CSVなど）を使用してデータを挿入できます。また、ストリーミングエンドポイントを使用して[!DNL Platform]にデータをリアルタイムで挿入できます。

このドキュメントでは、バッチ取り込みの監視、部分的なバッチ取り込みエラーの管理、および部分的なバッチ取り込みタイプの参照に関する情報を提供します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
- [[!DNL Adobe Experience Platform Data Ingestion]](../home.md):データの送信先のメソッド [!DNL Experience Platform]。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Schema Registry]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

## エラー診断{#download-diagnostics}をダウンロード中

Adobe Experience Platformでは、入力ファイルのエラー診断をダウンロードできます。 診断は[!DNL Platform]内に最大30日間保持されます。

### リスト入力ファイル{#list-files}

次の要求は、ファイナライズされたバッチで提供されるすべてのファイルのリストを取得します。

**API 形式**

```shell
GET /batches/{BATCH_ID}/meta?path=input_files
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 検索するバッチの ID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/af838510-2233-11ea-acf0-f3edfcded2d2/meta?path=input_files \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、診断が保存された場所の詳細を示すJSONオブジェクトが返されます。

```json
{
    "_page": {
        "count": 1,
        "limit": 100
    },
    "data": [
        {
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/export/batches/af838510-2233-11ea-acf0-f3edfcded2d2/meta?path=input_files/fileMetaData1.json"
                }
            },
            "length": "1337",
            "name": "fileMetaData1.json"
        },
                {
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/export/batches/af838510-2233-11ea-acf0-f3edfcded2d2}/meta?path=input_files/fileMetaData2.json"
                }
            },
            "length": "1042",
            "name": "fileMetaData2.json"
        }
    ]
}
```

### 入力ファイルの診断を取得{#retrieve-diagnostics}

すべての異なる入力ファイルのリストを取得したら、次のリクエストを使用して、個々のファイルの診断を取得できます。

**API 形式**

```shell
GET /batches/{BATCH_ID}/meta?path=input_files/{FILE}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 検索するバッチの ID。 |
| `{FILE}` | アクセスするファイルの名前。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/af838510-2233-11ea-acf0-f3edfcded2d2/meta?path=input_files/fileMetaData1.json \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、診断が保存された場所を詳細に示す`path`オブジェクトを含むJSONオブジェクトが返されます。 応答は、[JSON行](https://jsonlines.org/)形式の`path`オブジェクトを返します。

```json
{"path": "F1.json"}
{"path": "etc/F2.json"}
```

## バッチインジェストエラーを取得{#retrieve-errors}

バッチにエラーが含まれる場合は、これらのエラーに関するエラー情報を取得して、データを再取り込みできるようにする必要があります。

### ステータスの確認 {#check-status}

取得したバッチのステータスを確認するには、GET リクエストのパスにバッチの ID を指定する必要があります。

**API 形式**

```http
GET /catalog/batches/{BATCH_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | ステータスを確認するバッチの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/catalog/batches/af838510-2233-11ea-acf0-f3edfcded2d2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**エラーのない応答**

成功した応答は、バッチのステータスに関する詳細情報と共に返されます。

```json
{
    "af838510-2233-11ea-acf0-f3edfcded2d2": {
        "status": "success",
        "tags": {
            "acp_enableErrorDiagnostics": true,
            "acp_partialIngestionPercent": 5
        },
        "relatedObjects": [
            {
                "type": "dataSet",
                "id": "5deac2648a19d218a888d2b1"
            }
        ],
        "id": "af838510-2233-11ea-acf0-f3edfcded2d2",
        "externalId": "af838510-2233-11ea-acf0-f3edfcded2d2",
        "inputFormat": {
            "format": "parquet"
        },
        "imsOrg": "{IMS_ORG}",
        "started": 1576741718543,
        "metrics": {
            "inputByteSize": 568,
            "inputFileCount": 4,
            "inputRecordCount": 519,
            "outputRecordCount": 497,
            "failedRecordCount": 0
        },
        "completed": 1576741722026,
        "created": 1576741597205,
        "createdClient": "{API_KEY}",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "updated": 1576741722644,
        "version": "1.0.5"
    }    
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `metrics.failedRecordCount` | 解析、変換、または検証の結果、処理できなかった行の数です。 この値は、`outputRecordCount`から`inputRecordCount`を引くことで得られます。 この値は、`errorDiagnostics`が有効になっているかどうかに関係なく、すべてのバッチで生成されます。 |

**エラーのある応答**

バッチに1つ以上のエラーがあり、エラー診断が有効になっている場合、応答は、ペイロード内およびダウンロード可能なエラーファイル内のエラーに関する詳細情報を返します。 エラーを含むバッチのステータスは、成功のステータスのままである場合があります。

```json
{
    "01E8043CY305K2MTV5ANH9G1GC": {
        "status": "success",
        "tags": {
            "acp_enableErrorDiagnostics": true,
            "acp_partialIngestionPercent": 5
        },
        "relatedObjects": [
            {
                "type": "dataSet",
                "id": "5deac2648a19d218a888d2b1"
            }
        ],
        "id": "01E8043CY305K2MTV5ANH9G1GC",
        "externalId": "01E8043CY305K2MTV5ANH9G1GC",
        "inputFormat": {
            "format": "parquet"
        },
        "imsOrg": "{IMS_ORG}",
        "started": 1576741718543,
        "metrics": {
            "inputByteSize": 568,
            "inputFileCount": 4,
            "inputRecordCount": 519,
            "outputRecordCount": 514,
            "failedRecordCount": 5
        },
        "completed": 1576741722026,
        "created": 1576741597205,
        "createdClient": "{API_KEY}",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "updated": 1576741722644,
        "version": "1.0.5",
        "errors": [
           {
             "code": "INGEST-1212-400",
             "description": "Encountered 5 errors in the data. Successfully ingested 514 rows. Please review the associated diagnostic files for more details."
           },
           {
             "code": "INGEST-1401-400",
             "description": "The row has corrupted data and cannot be read or parsed. Fix the corrupted data and try again.",
             "recordCount": 2
           },
           {
             "code": "INGEST-1555-400",
             "description": "A required field is either missing or has a value of null. Add the required field to the input row and try again.",
             "recordCount": 3
           }
        ]
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `metrics.failedRecordCount` | 解析、変換、または検証の結果、処理できなかった行の数です。 この値は、`outputRecordCount`から`inputRecordCount`を引くことで得られます。 この値は、`errorDiagnostics`が有効になっているかどうかに関係なく、すべてのバッチで生成されます。 |
| `errors.recordCount` | 指定したエラーコードが失敗した行数です。 `errorDiagnostics`が有効な場合、この値は&#x200B;****&#x200B;のみ生成されます。 |

>[!NOTE]
>
>エラー診断が使用できない場合は、代わりに次のエラーメッセージが表示されます。
>
```json
>{
>       "errors": [{
>               "code": "INGEST-1211-400",
>               "description": "Encountered errors while parsing, converting or otherwise validating the data. Please resend the data with error diagnostics enabled to collect additional information on failure types"
>       }]
>}
>```

## 次の手順 {#next-steps}

このチュートリアルでは、部分的なバッチ取り込みエラーを監視する方法について説明しました。 バッチ取得について詳しくは、『[バッチ取得開発者ガイド](../batch-ingestion/api-overview.md)』を参照してください。

## 付録 {#appendix}

ここでは、取り込みエラータイプに関する補足情報を提供します。

### 部分バッチ取得エラータイプ {#partial-ingestion-types}

部分的なバッチ取り込みでは、データを取り込む際に次の3つの異なるエラータイプが発生します。

- [読み取り不能なファイル](#unreadable)
- [無効なスキーマまたはヘッダー](#schemas-headers)
- [解析不可の行](#unparsable)

### 読み取り不能なファイル {#unreadable}

取得されたバッチに読み取り不可能なファイルが含まれている場合、バッチのエラーはバッチ自体に添付されます。失敗したバッチについて詳しくは、『[失敗したバッチの取得ガイド](../quality/retrieve-failed-batches.md)』を参照してください。

### 無効なスキーマまたはヘッダー {#schemas-headers}

取得されたバッチに無効なスキーマまたは無効なヘッダーが含まれている場合、バッチのエラーはバッチ自体に添付されます。失敗したバッチについて詳しくは、『[失敗したバッチの取得ガイド](../quality/retrieve-failed-batches.md)』を参照してください。

### 解析不可の行 {#unparsable}

取り込んだバッチに解析できない行がある場合は、次の要求を使用して、エラーを含むファイルのリストを表示できます。

**API 形式**

```http
GET /export/batches/{BATCH_ID}/meta?path=row_errors
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | エラー情報を取得するバッチの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/01EFZ7W203PEKSAMVJC3X99VHQ/meta?path=row_errors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、エラーのあるファイルのリストが返されます。

```json
{
    "data": [
        {
            "name": "conversion_errors_0.json",
            "length": "1162",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/export/batches/01EFZ7W203PEKSAMVJC3X99VHQ/meta?path=row_errors%2Fconversion_errors_0.json"
                }
            }
        },
        {
            "name": "parsing_errors_0.json",
            "length": "153",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/export/batches/01EFZ7W203PEKSAMVJC3X99VHQ/meta?path=row_errors%2Fparsing_errors_0.json"
                }
            }
        }
    ],
    "_page": {
        "limit": 100,
        "count": 2
    }
}
```

その後、[診断の取得エンドポイント](#retrieve-diagnostics)を使用して、エラーに関する詳細な情報を取得できます。

エラーファイルの取得に関する応答の例を次に示します。

```json
{
    "_corrupt_record": "{missingQuotes: 'v1'}",
    "_errors": [{
        "code": "1401",
        "message": "Row is corrupted and cannot be read, please fix and resend."
    }],
    "_filename": "parsing_errors_0.json"
}
```
