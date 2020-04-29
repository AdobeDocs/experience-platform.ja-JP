---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform部分的なバッチ取り込みの概要
topic: overview
translation-type: tm+mt
source-git-commit: d560e8dd07e9590376728ae6575766cc382325a5

---



# 部分的なバッチインジェスト（ベータ）

部分的なバッチ取り込みは、エラーを含むデータを特定のしきい値まで取り込む機能です。 この機能を使用すると、ユーザーは正しいデータをすべてAdobe Experience Platformに正しく取り込み、間違ったデータはすべて別々にバッチ処理され、その理由の詳細も表示できます。

このドキュメントでは、部分的なバッチ取り込みを管理するためのチュートリアルを提供します。

さらに、このチュートリ [アルの付録](#appendix) では、部分的なバッチ取り込みエラータイプの参照を示します。

>[!IMPORTANT] この機能はAPIを使用してのみ存在します。 この機能にアクセスするには、チームにお問い合わせください。

## はじめに

このチュートリアルでは、部分的なバッチ取り込みに関連する様々なAdobe Experience Platformサービスに関する実用的な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [バッチインジェスト](./overview.md):CSVやParketなどのデータファイルからデータを取り込んで保存する方法。
- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。

以下の節では、プラットフォームAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

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

## APIでの部分的なバッチ取り込みのデータセットの有効化

<!-- >[!NOTE] This section describes enabling a dataset for partial batch ingestion using the API. For instructions on using the UI, please read the [enable a dataset for partial batch ingestion in the UI](#enable-a-dataset-for-partial-batch-ingestion-in-the-ui) step. -->

新しいデータセットを作成したり、部分的な取り込みが有効な既存のデータセットを変更したりできます。

新しいデータセットを作成するには、「データセットの作成」の手 [順に従ってください](../../catalog/api/create-dataset.md)。 「データセットの作 *成」の手順に* 、リクエスト本文に次のフィールドを追加します。

```json
{
    ...
    "tags" : {
        "partialBatchIngestion":["errorThresholdPercentage:5"]
    },
    ...
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `errorThresholdPercentage` | バッチ全体が失敗する前の許容エラーの割合。 |

同様に、既存のデータセットを変更するには、カタログ開発ガイドの手 [順に従います](../../catalog/api/update-object.md)。

データセット内に、上記のタグを追加する必要があります。

<!-- ## Enable a dataset for partial batch ingestion in the UI

>[!NOTE] This section describes enabling a dataset for partial batch ingestion using the UI. If you have already enabled a dataset for partial batch ingestion using the API, you can skip ahead to the next section.

To enable a dataset for partial ingestion through the Platform UI, click **Datasets** in the left navigation. You can either [create a new dataset](#create-a-new-dataset-with-partial-batch-ingestion-enabled) or [modify an existing dataset](#modify-an-existing-dataset-to-enable-partial-batch-ingestion).

### Create a new dataset with partial batch ingestion enabled

To create a new dataset, follow the steps in the [dataset user guide](../../catalog/datasets/user-guide.md). Once you reach the *Configure dataset* step, take note of the *Partial Ingestion* and *Error Diagnostics* fields.

![](../images/batch-ingestion/partial-ingestion/configure-dataset-focus.png)

The *Partial ingestion* toggle allows you to enable or disable the use of partial batch ingestion.

The *Error Diagnostics* toggle only appears when the *Partial Ingestion* toggle is off. This feature allows Platform to generate detailed error messages about your ingested batches. If the *Partial Ingestion* toggle is turned on, enhanced error diagnostics are automatically enforced.

![](../images/batch-ingestion/partial-ingestion/configure-dataset-partial-ingestion-focus.png)

The *Error threshold* allows you to set the percentage of acceptable errors before the entire batch will fail. By default, this value is set to 5%.

### Modify an existing dataset to enable partial batch ingestion

To modify an existing dataset, select the dataset you want to modify. The sidebar on the right populates with information about the dataset. 

![](../images/batch-ingestion/partial-ingestion/modify-dataset-focus.png)

The *Partial ingestion* toggle allows you to enable or disable the use of partial batch ingestion.

The *Error threshold* allows you to set the percentage of acceptable errors before the entire batch will fail. By default, this value is set to 5%. -->

## 部分的なバッチ取り込みエラーの取得

バッチにエラーが含まれる場合は、これらのエラーに関するエラー情報を取得して、データを再取り込みできるようにする必要があります。

### ステータスの確認

取り込んだバッチのステータスを確認するには、GET要求のパスにバッチのIDを指定する必要があります。

**API形式**

```http
GET /catalog/batches/{BATCH_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | ステ `id` ータスを確認するバッチの値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/catalog/batches/{BATCH_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス200とバッチのステータスに関する詳細情報を返します。

```json
{
    "af838510-2233-11ea-acf0-f3edfcded2d2": {
        "status": "success",
        "tags": {
            ...
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
            "outputRecordCount": 497
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

バッチにエラーがあり、エラー診断が有効になっている場合、ステータスは「成功」になり、ダウンロード可能なエラーファイルで提供されたエラーの詳細が表示されます。

## 次の手順

このチュートリアルでは、部分的なバッチ取り込みを有効にするデータセットの作成または変更方法について説明しました。 バッチインジェストの詳細については、『バッチインジェスト開発者ガ [イド』を参照してくださ](./api-overview.md)い。

## 部分的なバッチ取り込みエラータイプ {#appendix}

部分的なバッチ取り込みでは、データを取り込む際に4つの異なるエラータイプが発生します。

- [読み取り不能なファイル](#unreadable)
- [無効なスキーマまたはヘッダー](#schemas-headers)
- [解析不可の行](#unparsable)
- [無効なXDM変換](#conversion)

### 読み取り不能なファイル {#unreadable}

取り込まれたバッチに読み取り不可能なファイルが含まれている場合、バッチのエラーはバッチ自体に添付されます。 失敗したバッチの取得の詳細については、失敗したバッチの取得ガイドを [参照してください](../quality/retrieve-failed-batches.md)。

### 無効なスキーマまたはヘッダー {#schemas-headers}

取り込まれたバッチに無効なスキーマまたは無効なヘッダが含まれている場合、バッチのエラーはバッチ自体に添付されます。 失敗したバッチの取得の詳細については、失敗したバッチの取得ガイドを [参照してください](../quality/retrieve-failed-batches.md)。

### 解析不可の行 {#unparsable}

取り込んだバッチに解析できない行が含まれている場合、バッチのエラーはファイルに保存され、以下に説明するエンドポイントを使用してアクセスできます。

**API形式**

```http
GET /export/batches/{BATCH_ID}/failed?path=parse_errors
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | エラ `id` ー情報を取得するバッチの値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/failed?path=parse_errors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、解析不可能な行の詳細を含むHTTPステータス200を返します。

```json
{
    "_corrupt_record":"{missingQuotes:"v1"}",
    "_errors": [{
         "code":"1401",
         "message":"Row is corrupted and cannot be read, please fix and resend."
    }],
    "_filename": "a1.json"
}
```

### 無効なXDM変換 {#conversion}

取り込んだバッチに無効なXDM変換が含まれている場合、バッチのエラーはファイルに保存され、次のエンドポイントを使用してアクセスできます。

**API形式**

```http
GET /export/batches/{BATCH_ID}/failed?path=conversion_errors
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | エラ `id` ー情報を取得するバッチの値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/failed?path=conversion_errors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、XDM変換の失敗の詳細を含むHTTPステータス200を返します。

```json
{
    "col1":"v1",
    "col2":"v2",
    "col3":[{
        "g1":"h1"
    }],
    "_errors":[{
        "column":"col3",
        "code":"123",
        "message":"Cannot convert array element from Object to String"
    }],
    "_filename":"a1.json"
},
{
    "col1":"v1",
    "col2":"v2",
    "col3":[{
        "g1":"h1"
    }],
    "_errors":[{
        "column":"col1",
        "code":"100",
        "message":"Cannot convert string to float"
    }],
    "_filename":"a2.json"
}
```
