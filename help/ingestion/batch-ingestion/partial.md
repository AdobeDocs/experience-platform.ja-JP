---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform部分的なバッチ取り込みの概要
topic: overview
translation-type: tm+mt
source-git-commit: d560e8dd07e9590376728ae6575766cc382325a5
workflow-type: tm+mt
source-wordcount: '795'
ht-degree: 2%

---



# 部分的なバッチインジェスト（ベータ版）

部分的なバッチ取り込みとは、エラーを含むデータを特定のしきい値まで取り込む機能です。 この機能を使用すると、ユーザーは正しいデータをすべてAdobe Experience Platformに正しく取り込むと同時に、誤ったデータをすべて個別にバッチ処理し、その理由を詳細にまとめることができます。

このドキュメントでは、部分的なバッチ取り込みを管理するためのチュートリアルを提供します。

また、このチュートリアルの [付録](#appendix) 、部分的なバッチ取り込みエラータイプのリファレンスも紹介します。

>[!IMPORTANT] この機能はAPIを使用した場合にのみ存在します。 この機能にアクセスするには、チームにお問い合わせください。

## はじめに

このチュートリアルでは、部分的なバッチ取り込みに関連する様々なAdobe Experience Platformサービスに関する実用的な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [バッチインジェスト](./overview.md): プラットフォームが、CSVやParketなどのデータファイルからデータを取り込んで保存する方法。
- [Experience Data Model(XDM)](../../xdm/home.md): プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。

以下の節では、プラットフォームAPIの呼び出しを正常に行うために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

## APIでの部分的なバッチ取り込みのデータセットの有効化

<!-- >[!NOTE] This section describes enabling a dataset for partial batch ingestion using the API. For instructions on using the UI, please read the [enable a dataset for partial batch ingestion in the UI](#enable-a-dataset-for-partial-batch-ingestion-in-the-ui) step. -->

部分的な取り込みが有効な場合は、新しいデータセットを作成したり、既存のデータセットを変更したりできます。

新しいデータセットを作成するには、「データセットの [作成」チュートリアルの手順に従い](../../catalog/api/create-dataset.md)ます。 「データセットの *作成* 」の手順に進んだら、リクエスト本文に次のフィールドを追加します。

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
| `errorThresholdPercentage` | バッチ全体が失敗する前の許容可能なエラーの割合。 |

同様に、既存のデータセットを変更するには、 [カタログ開発ガイドの手順に従います](../../catalog/api/update-object.md)。

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

## 部分的なバッチインジェストエラーの取得

バッチにエラーが含まれる場合は、これらのエラーに関するエラー情報を取得して、データを再取り込みできるようにする必要があります。

### ステータスの確認

取り込んだバッチのステータスを確認するには、GET要求のパスにバッチのIDを指定する必要があります。

**API形式**

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

**応答**

正常な応答は、バッチのステータスに関する詳細情報と共にHTTPステータス200を返します。

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

バッチにエラーがあり、エラー診断が有効になっている場合、ステータスは「成功」になり、ダウンロード可能なエラーファイルで提供されるエラーの詳細についての情報が表示されます。

## 次の手順

このチュートリアルでは、データセットを作成または変更して部分的なバッチ取り込みを有効にする方法について説明しました。 バッチインジェストの詳細については、『 [バッチインジェスト開発者ガイド](./api-overview.md)』を参照してください。

## 部分的なバッチ取り込みエラータイプ {#appendix}

部分的なバッチ取り込みでは、データを取り込むときに4つの異なるエラータイプがあります。

- [読み取り不可能なファイル](#unreadable)
- [無効なスキーマまたはヘッダー](#schemas-headers)
- [解析不可の行](#unparsable)
- [無効なXDM変換](#conversion)

### 読み取り不可能なファイル {#unreadable}

取り込まれたバッチに読み取り不可能なファイルが含まれている場合、バッチのエラーはバッチ自体に添付されます。 失敗したバッチの取得について詳しくは、失敗したバッチの [取得ガイドを参照してください](../quality/retrieve-failed-batches.md)。

### 無効なスキーマまたはヘッダー {#schemas-headers}

取り込まれたバッチに無効なスキーマまたは無効なヘッダが含まれる場合、バッチのエラーはバッチ自体に添付されます。 失敗したバッチの取得について詳しくは、失敗したバッチの [取得ガイドを参照してください](../quality/retrieve-failed-batches.md)。

### 解析不可の行 {#unparsable}

取り込まれたバッチに解析できない行が含まれる場合、バッチのエラーはファイルに保存され、以下に説明するエンドポイントを使用してアクセスできます。

**API形式**

```http
GET /export/batches/{BATCH_ID}/failed?path=parse_errors
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | エラー情報を取得するバッチの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/failed?path=parse_errors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、解析不可能な行の詳細を含むHTTPステータス200が返されます。

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

取り込まれたバッチに無効なXDM変換が含まれる場合、バッチのエラーはファイルに保存され、次のエンドポイントを使用してアクセスできます。

**API形式**

```http
GET /export/batches/{BATCH_ID}/failed?path=conversion_errors
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | エラー情報を取得するバッチの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/failed?path=conversion_errors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答が返されると、XDM変換の失敗の詳細と共にHTTPステータス200が返されます。

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
