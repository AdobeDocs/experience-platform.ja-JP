---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform部分的なバッチ取り込みの概要
topic: overview
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a

---



# 部分的なバッチ取り込み

部分的なバッチ取り込みは、エラーを含むデータを特定のしきい値まで取り込む機能です。 この機能を使用すると、ユーザーは正しいデータをすべてAdobe Experience Platformに正しく取り込み、間違ったデータはすべて別々にバッチ処理され、その理由の詳細も表示できます。

このドキュメントでは、部分的なバッチ取り込みを管理するためのチュートリアルを提供します。

さらに、このチュートリ [アルの付録](#partial-batch-ingestion-error-types) では、部分的なバッチ取り込みエラータイプの参照を示します。

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

>[!NOTE] この節では、APIを使用した部分的なバッチ取り込みのデータセットの有効化について説明します。 UIの使用手順については、UIの手順で部分的なバッ [チ取り込みのデータセットを有効にするを参照してください](#enable-a-dataset-for-partial-batch-ingestion-in-the-ui) 。

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

## UIでの部分的なバッチ取り込みのデータセットの有効化

>[!NOTE] この節では、UIを使用した部分的なバッチ取り込みのデータセットの有効化について説明します。 APIを使用して部分的なバッチ取り込みのデータセットを既に有効にしている場合は、次のセクションに進んでください。

プラットフォームUIから部分的な取り込みのためのデータセットを有効にするには、左のナビゲ **ーションで** 「Datasets」をクリックします。 新しいデータセッ [トを作成するか](#create-a-new-dataset-with-partial-batch-ingestion-enabled) 、既存の [データセットを変更できます](#modify-an-existing-dataset-to-enable-partial-batch-ingestion)。

### 部分的なバッチ取り込みが有効な新しいデータセットの作成

新しいデータセットを作成するには、『データセットユーザガイド』の手 [順に従ってくださ](../../catalog/datasets/user-guide.md)い。 データセットの設定手順 *に* 、「部分的なインジェスト *」フィールドと「エ* ラー診断 *」フィールド* を控えておきます。

![](../images/batch-ingestion/partial-ingestion/configure-dataset-focus.png)

[部分 *的な取り込み* ]切り替えを使用すると、部分的なバッチ取り込みの使用を有効または無効にできます。

[エラ *ー診断* ]切り替えは、[部分的なインジェスト ** ]切り替えがオフの場合にのみ表示されます。 この機能を使用すると、取り込んだバッチに関する詳細なエラーメッセージをプラットフォームで生成できます。 [部分的なインジェ *スト* ]切り替えがオンになっている場合は、拡張されたエラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/configure-dataset-partial-ingestion-focus.png)

エラー *しきい値を使用すると* 、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。 デフォルトでは、この値は5%に設定されています。

### 既存のデータセットを変更して、部分的なバッチ取り込みを有効にする

既存のデータセットを変更するには、変更するデータセットを選択します。 右側のサイドバーに、データセットに関する情報が表示されます。

![](../images/batch-ingestion/partial-ingestion/modify-dataset-focus.png)

[部分 *的な取り込み* ]切り替えを使用すると、部分的なバッチ取り込みの使用を有効または無効にできます。

エラー *しきい値を使用すると* 、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。 デフォルトでは、この値は5%に設定されています。

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

## 部分的なバッチ取り込みエラータイプ

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
