---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform 部分取得の概要
topic: overview
translation-type: tm+mt
source-git-commit: df6a6e20733953a0983bbfdf66ca2abc6f03e977
workflow-type: tm+mt
source-wordcount: '1420'
ht-degree: 42%

---



# 部分バッチ取得

部分バッチ取得は、エラーを含むデータを特定のしきい値まで取得する機能です。この機能を使用すると、正しいデータをすべて Adobe Experience Platform に正しく取得し、間違ったデータはその理由の詳細とともにすべて別々にバッチ処理されます。

このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。

さらに、このチュートリアルの[付録](#appendix)では、部分バッチ取得エラータイプの参照を示します。

## はじめに

このチュートリアルでは、部分バッチ取得に関わる様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [バッチインジェスト](./overview.md): CSVやParketなどのデータファイルからデータを [!DNL Platform] 取り込んで保存する方法。
- [[!DNL Experience Data Model] (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

The following sections provide additional information that you will need to know in order to successfully make calls to [!DNL Platform] APIs.

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

## Enable a batch for partial batch ingestion in the API {#enable-api}

>[!NOTE]
>
>この節では、APIを使用した部分的なバッチ取り込みに対してバッチを有効にする方法を説明します。 For instructions on using the UI, please read the [enable a batch for partial batch ingestion in the UI](#enable-ui) step.

部分的な取り込みが有効な新しいバッチを作成できます。

新しいバッチを作成するには、『 [バッチインジェスト開発者ガイド](./api-overview.md)』の手順に従います。 Once you reach the **[!UICONTROL Create batch]** step, add the following field within the request body:

```json
{
    "enableErrorDiagnostics": true,
    "partialIngestionPercentage": 5
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `enableErrorDiagnostics` | バッチに関する詳細なエラーメッセージ [!DNL Platform] を生成するためのフラグ。 |
| `partialIngestionPercentage` | バッチ全体が失敗する前の許容エラー率。したがって、この例では、バッチが失敗する前に、最大5%のエラーが発生する可能性があります。 |


## Enable a batch for partial batch ingestion in the UI {#enable-ui}

>[!NOTE]
>
>この節では、UIを使用した部分的なバッチ取り込みに対してバッチを有効にする方法について説明します。 APIを使用してバッチの部分的な取り込みを既に有効にしている場合は、次のセクションに進むことができます。

バッチを [!DNL Platform] UIから部分的に取り込むために有効にするには、ソース接続を介して新しいバッチを作成するか、既存のデータセットに新しいバッチを作成するか、「[!UICONTROL CSVをXDMフローにマップ]」を介して新しいバッチを作成します。

### 新しいソース接続の作成 {#new-source}

新しいソース接続を作成するには、 [ソースの概要に記載されている手順に従います](../../sources/home.md)。 Once you reach the **[!UICONTROL Dataflow detail]** step, take note of the **[!UICONTROL Partial ingestion]** and **[!UICONTROL Error diagnostics]** fields.

![](../images/batch-ingestion/partial-ingestion/configure-batch.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

The **[!UICONTROL Error diagnostics]** toggle only appears when the **[!UICONTROL Partial ingestion]** toggle is off. This feature allows [!DNL Platform] to generate detailed error messages about your ingested batches. If the *[!UICONTROL Partial ingestion]* toggle is turned on, enhanced error diagnostics are automatically enforced.

![](../images/batch-ingestion/partial-ingestion/configure-batch-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

### 既存のデータセットを使用する {#existing-dataset}

既存のデータセットを使用するには、開始がデータセットを選択します。 右側のサイドバーに、データセットに関する情報が表示されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

The **[!UICONTROL Error diagnostics]** toggle only appears when the **[!UICONTROL Partial ingestion]** toggle is off. This feature allows [!DNL Platform] to generate detailed error messages about your ingested batches. If the **[!UICONTROL Partial ingestion]** toggle is turned on, enhanced error diagnostics are automatically enforced.

![](../images/batch-ingestion/partial-ingestion/monitor-dataset-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

これで、 **追加「** data」ボタンを使用してデータをアップロードでき、部分的な取り込みを使用して取り込むことができます。

### 「[!UICONTROL Map CSV to XDMスキーマ]」フローの使用 {#map-flow}

「[!UICONTROL CSVをXDMスキーマに]マップ [」のフローを使用するには、「CSVファイルを](../tutorials/map-a-csv-file.md)マップ」のチュートリアルに示されている手順に従います。 Once you reach the **[!UICONTROL Add data]** step, take note of the **[!UICONTROL Partial ingestion]** and **[!UICONTROL Error diagnostics]** fields.

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

The **[!UICONTROL Error diagnostics]** toggle only appears when the **[!UICONTROL Partial ingestion]** toggle is off. This feature allows [!DNL Platform] to generate detailed error messages about your ingested batches. If the **[!UICONTROL Partial ingestion]** toggle is turned on, enhanced error diagnostics are automatically enforced.

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

## ファイルレベルのメタデータのダウンロード {#download-metadata}

Adobe Experience Platformでは、入力ファイルのメタデータのダウンロードが許可されます。 メタデータは30日以内に保持 [!DNL Platform] されます。

### リスト入力ファイル {#list-files}

次の要求を行うと、ファイナライズされたバッチで提供されるすべてのファイルのリストを表示できます。

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/meta?path=input_files \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、メタデータが保存された場所を詳細に示すパスオブジェクトが含まれたJSONオブジェクトを持つHTTPステータス200が返されます。

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
                    "href": "https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/meta?path=input_files/fileMetaData1.json"
                }
            },
            "length": "1337",
            "name": "fileMetaData1.json"
        },
                {
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/meta?path=input_files/fileMetaData2.json"
                }
            },
            "length": "1042",
            "name": "fileMetaData2.json"
        }
    ]
}
```

### 入力ファイルのメタデータを取得 {#retrieve-metadata}

すべての様々な入力ファイルのリストを取得したら、次のエンドポイントを使用して、個々のファイルのメタデータを取得できます。

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/meta?path=input_files/fileMetaData1.json \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、メタデータが保存された場所を詳細に示すパスオブジェクトが含まれたJSONオブジェクトを持つHTTPステータス200が返されます。

```json
{"path": "F1.json"}
{"path": "etc/F2.json"}
```

## 部分バッチ取得エラーの獲得 {#retrieve-errors}

バッチにエラーが含まれる場合は、これらのエラーに関するエラー情報を獲得して、データを再取得できるようにする必要があります。

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
curl -X GET https://platform.adobe.io/data/foundation/catalog/batches/{BATCH_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**エラーのない応答**

正常な応答は、HTTP ステータス 200 とバッチのステータスに関する詳細情報を返します。

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
| `metrics.failedRecordCount` | 解析、変換、または検証の結果、処理できなかった行の数です。 この値は、から値を引くことで得ら `inputRecordCount` れ `outputRecordCount`ます。 この値は、有効になっている場合に関係なく、すべてのバッチで生成 `errorDiagnostics` されます。 |

**エラーのある応答**

バッチに1つ以上のエラーがあり、エラー診断が有効になっている場合、ステータスには、応答内とダウンロード可能なエラーファイル内の両方で提供されるエラーに関する詳細情報が表示されます。 `success`

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
| `metrics.failedRecordCount` | 解析、変換、または検証の結果、処理できなかった行の数です。 この値は、から値を引くことで得ら `inputRecordCount` れ `outputRecordCount`ます。 この値は、有効になっている場合に関係なく、すべてのバッチで生成 `errorDiagnostics` されます。 |
| `errors.recordCount` | 指定したエラーコードが失敗した行数です。 この値は **、が有効な場合**`errorDiagnostics` にのみ生成されます。 |

>[!NOTE]
>
>エラー診断が使用できない場合は、代わりに次のエラーメッセージが表示されます。
> 
```json
> {
>         "errors": [{
>                 "code": "INGEST-1211-400",
>                 "description": "Encountered errors while parsing, converting or otherwise validating the data. Please resend the data with error diagnostics enabled to collect additional information on failure types"
>         }]
> }
> ```

## 次の手順 {#next-steps}

このチュートリアルでは、部分バッチ取得を有効にするデータセットの作成または変更方法について説明しました。バッチ取得について詳しくは、『[バッチ取得開発者ガイド](./api-overview.md)』を参照してください。

## 部分バッチ取得エラータイプ {#appendix}

部分的なバッチ取り込みでは、データを取り込むときに3つの異なるエラータイプがあります。

- [読み取り不能なファイル](#unreadable)
- [無効なスキーマまたはヘッダー](#schemas-headers)
- [解析不可の行](#unparsable)

### 読み取り不能なファイル {#unreadable}

取得されたバッチに読み取り不可能なファイルが含まれている場合、バッチのエラーはバッチ自体に添付されます。失敗したバッチについて詳しくは、『[失敗したバッチの取得ガイド](../quality/retrieve-failed-batches.md)』を参照してください。

### 無効なスキーマまたはヘッダー {#schemas-headers}

取得されたバッチに無効なスキーマまたは無効なヘッダーが含まれている場合、バッチのエラーはバッチ自体に添付されます。失敗したバッチについて詳しくは、『[失敗したバッチの取得ガイド](../quality/retrieve-failed-batches.md)』を参照してください。

### 解析不可の行 {#unparsable}

取得されたバッチに解析できない行が含まれている場合、バッチのエラーはファイルに保存され、以下に説明するエンドポイントを使用してアクセスできます。

**API 形式**

```http
GET /export/batches/{BATCH_ID}/meta?path=row_errors
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | エラー情報を取得するバッチの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/meta?path=row_errors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、HTTP ステータス 200 と解析不可能な行の詳細をを返します。

```json
{
    "_corrupt_record": "{missingQuotes:"v1"}",
    "_errors": [{
         "code": "1401",
         "message": "Row is corrupted and cannot be read, please fix and resend."
    }],
    "_filename": "a1.json"
}
```
