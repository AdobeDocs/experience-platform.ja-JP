---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformの部分バッチ取り込みの概要
topic: overview
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '1237'
ht-degree: 1%

---



# 部分的なバッチ取り込み

部分的なバッチ取り込みとは、エラーを含むデータを特定のしきい値まで取り込む機能です。 この機能を使用すると、正しいデータをすべてAdobe Experience Platformに取り込むと同時に、誤ったデータをすべて個別にバッチ処理し、その理由を詳細にまとめることができます。

このドキュメントでは、部分的なバッチ取り込みを管理するためのチュートリアルを提供します。

また、このチュートリアルの [付録](#appendix) 、部分的なバッチ取り込みエラータイプのリファレンスも紹介します。

## はじめに

このチュートリアルでは、部分的なバッチ取り込みに関連する様々なAdobe Experience Platformサービスに関する実用的な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [バッチインジェスト](./overview.md): CSVやParketなどのデータファイルからデータを [!DNL Platform] 取り込んで保存する方法。
- [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

以下の節では、APIの呼び出しを正常に行うために知っておく必要がある追加情報について説明し [!DNL Platform] ます。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

## APIでの部分的なバッチ取り込みに対するバッチの有効化 {#enable-api}

>[!NOTE]
>
>この節では、APIを使用した部分的なバッチ取り込みに対してバッチを有効にする方法を説明します。 UIの使用方法については、UIの手順でバッチの部分的な取り込みを [有効にするを読んでください](#enable-ui) 。

部分的な取り込みが有効な新しいバッチを作成できます。

新しいバッチを作成するには、『 [バッチインジェスト開発者ガイド](./api-overview.md)』の手順に従います。 バッチ *作成ステップに到達したら* 、リクエスト本文に次のフィールドを追加します。

```json
{
    ...
    "enableErrorDiagnostics": true,
    "partialIngestionPercentage": 5
    ...
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `enableErrorDiagnostics` | バッチに関する詳細なエラーメッセージ [!DNL Platform] を生成するためのフラグ。 |
| `partialIngestionPercentage` | バッチ全体が失敗する前の許容可能なエラーの割合。 したがって、この例では、バッチが失敗する前に、最大5%のエラーが発生する可能性があります。 |


## UIでの部分的なバッチ取り込みに対するバッチの有効化 {#enable-ui}

>[!NOTE]
>
>この節では、UIを使用した部分的なバッチ取り込みに対してバッチを有効にする方法について説明します。 APIを使用してバッチの部分的な取り込みを既に有効にしている場合は、次のセクションに進むことができます。

バッチを [!DNL Platform] UIから部分的に取り込むために有効にするには、ソース接続を介して新しいバッチを作成するか、既存のデータセットに新しいバッチを作成するか、「[!UICONTROL CSVをXDMフローにマップ]」を介して新しいバッチを作成します。

### 新しいソース接続の作成 {#new-source}

新しいソース接続を作成するには、 [ソースの概要に記載されている手順に従います](../../sources/home.md)。 デー *[!UICONTROL タフローの詳細]* ・ステップに進んだら、「 *[!UICONTROL 部分的なインジェスト]* 」フィールドと「 *[!UICONTROL エラー診断]* 」フィールドに注意してください。

![](../images/batch-ingestion/partial-ingestion/configure-batch.png)

「 *[!UICONTROL 部分インジェスト]* 」切り替えを使用すると、部分バッチインジェストの使用を有効または無効にできます。

[ *[!UICONTROL エラー診断]* ]トグルは、[ *[!UICONTROL 部分的なインジェスト]* ]トグルがオフの場合にのみ表示されます。 この機能を使用すると、取り込んだバッチ [!DNL Platform] に関する詳細なエラーメッセージを生成できます。 [ *[!UICONTROL 部分的な取り込み]* ]トグルがオンになっている場合は、拡張されたエラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/configure-batch-partial-ingestion-focus.png)

エラーのしきい値 *[!UICONTROL (]* Error threshold)を使用すると、バッチ全体が失敗する前に、許容できるエラーの割合を設定できます。 デフォルトでは、この値は5%に設定されています。

### 既存のデータセットの使用 {#existing-dataset}

既存のデータセットを使用するには、開始がデータセットを選択します。 右側のサイドバーには、データセットに関する情報が表示されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset.png)

「 *[!UICONTROL 部分インジェスト]* 」切り替えを使用すると、部分バッチインジェストの使用を有効または無効にできます。

[ *[!UICONTROL エラー診断]* ]トグルは、[ *[!UICONTROL 部分的なインジェスト]* ]トグルがオフの場合にのみ表示されます。 この機能を使用すると、取り込んだバッチ [!DNL Platform] に関する詳細なエラーメッセージを生成できます。 [ *[!UICONTROL 部分的な取り込み]* ]トグルがオンになっている場合は、拡張されたエラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset-partial-ingestion-focus.png)

エラーのしきい値 *[!UICONTROL (]* Error threshold)を使用すると、バッチ全体が失敗する前に、許容できるエラーの割合を設定できます。 デフォルトでは、この値は5%に設定されています。

これで、 **追加「** data」ボタンを使用してデータをアップロードでき、部分的な取り込みを使用して取り込むことができます。

### 「[!UICONTROL Map CSV to XDMスキーマ]」フローの使用 {#map-flow}

「[!UICONTROL CSVをXDMスキーマに]マップ [」のフローを使用するには、「CSVファイルを](../tutorials/map-a-csv-file.md)マップ」のチュートリアルに示されている手順に従います。 デ *[!UICONTROL ー追加タ]* 手順に進んだら、 *[!UICONTROL 部分的な取り込み]* と *[!UICONTROL エラー診断]* のフィールドに注意してください。

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow.png)

「 *[!UICONTROL 部分インジェスト]* 」切り替えを使用すると、部分バッチインジェストの使用を有効または無効にできます。

[ *[!UICONTROL エラー診断]* ]トグルは、[ *[!UICONTROL 部分的なインジェスト]* ]トグルがオフの場合にのみ表示されます。 この機能を使用すると、取り込んだバッチ [!DNL Platform] に関する詳細なエラーメッセージを生成できます。 [ *[!UICONTROL 部分的な取り込み]* ]トグルがオンになっている場合は、拡張されたエラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow-partial-ingestion-focus.png)

エラーのしきい値 *[!UICONTROL (]* Error threshold)を使用すると、バッチ全体が失敗する前に、許容できるエラーの割合を設定できます。 デフォルトでは、この値は5%に設定されています。

## 部分的なバッチインジェストエラーの取得 {#retrieve-errors}

バッチにエラーが含まれる場合は、これらのエラーに関するエラー情報を取得して、データを再取り込みできるようにする必要があります。

### ステータスの確認 {#check-status}

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
            "acp_enableErrorDiagnostics": true,
            "acp_partialIngestionPercent": 5
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

## 次の手順 {#next-steps}

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
