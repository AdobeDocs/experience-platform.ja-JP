---
keywords: Experience Platform;download scores;customer ai;popular topics
solution: Experience Platform
title: 顧客AIでのスコアのダウンロード
topic: Downloading scores
translation-type: tm+mt
source-git-commit: 7c892d92a50312fb4b733431737b796651689804

---


# 顧客AIでのスコアのダウンロード

このドキュメントは、顧客AIのスコアをダウンロードする際のガイドとして機能します。

## はじめに

顧客AIでは、スコアをパーケファイル形式でダウンロードできます。 このチュートリアルでは、はじめに [](../getting-started.md) 「Customer AIスコアのダウンロード」の節を読み終えている必要があります。

さらに、顧客AIのスコアにアクセスするには、正常な実行ステータスのサービスインスタンスが使用可能である必要があります。 新しいサービスインスタンスを作成するには、「顧客AIインスタンスの [設定](./configure.md)」にアクセスします。 最近サービスインスタンスを作成したが、まだトレーニングとスコアリングを受けている場合は、24時間待ってから実行を終了してください。

現在、顧客のAIスコアをダウンロードする方法は2つあります。

1. 個々のレベルでスコアをダウンロードしたい場合、またはリアルタイム顧客プロファイルが有効になっていない場合は、開始に進んでデータセットIDを [探してください](#dataset-id)。
2. プロファイルが有効になっていて、Customer AIを使用して設定したセグメントをダウンロードする場合は、Customer AIを使用して設定したセグメントを [ダウンロードします](#segment)。

## Find your dataset ID {#dataset-id}

顧客AIインサイトのサービスインスタンス内で、右上のナビゲーションの「 *その他のアクション* 」ドロップダウンをクリックし、「」を選択し **[!UICONTROL Access scores]**&#x200B;ます。

![その他のアクション](../images/insights/more-actions.png)

新しいダイアログが表示され、ダウンロードスコアに関するドキュメントへのリンクと現在のインスタンスのデータセットIDが含まれます。 データセットIDをクリップボードにコピーし、次の手順に進みます。

![データセットID](../images/download-scores/access-scores.png)

## バッチIDの取得 {#retrieve-your-batch-id}

前の手順のデータセットIDを使用して、バッチIDを取得するには、カタログAPIを呼び出す必要があります。 組織に属するバッチのリストではなく、成功した最新のバッチを返すために、このAPI呼び出しに追加のクエリパラメーターが使用されます。 追加のバッチを返すには、limitクエリーパラメーターの値を、返す金額に増やします。 使用可能なクエリパラメーターのタイプの詳細については、クエリパラメーターを使用したカタログデータの [フィルタリングに関するガイドを参照してください](../../../catalog/api/filter-data.md)。

**API形式**

```http
GET /batches?&dataSet={DATASET_ID}&createdClient=acp_foundation_push&status=success&orderBy=desc:created&limit=1
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | データセットIDは、「アクセススコア」ダイアログで使用できます。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/catalog/batches?dataSet=5cd9146b31dae914b75f654f&createdClient=acp_foundation_push&status=success&orderBy=desc:created&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、バッチIDオブジェクトを含むペイロードを返します。 この例では、返されるオブジェクトのキー値はバッチIDで `01E5QSWCAASFQ054FNBKYV6TIQ`す。 バッチIDをコピーして、次のAPI呼び出しで使用します。

```json
{
    "01E5QSWCAASFQ054FNBKYV6TIQ": {
        "status": "success",
        "tags": {
            "Tags": [ ... ],
        },
        "relatedObjects": [
            {
                "type": "dataSet",
                "id": "5cd9146b31dae914b75f654f"
            }
        ],
        "id": "01E5QSWCAASFQ054FNBKYV6TIQ",
        "externalId": "01E5QSWCAASFQ054FNBKYV6TIQ",
        "replay": {
            "predecessors": [
                "01E5N7EDQQP4JHJ93M7C3WM5SP"
            ],
            "reason": "Replacing for 2020-04-09",
            "predecessorListingType": "IMMEDIATE"
        },
        "inputFormat": {
            "format": "parquet"
        },
        "imsOrg": "412657965Y566A4A0A495D4A@AdobeOrg",
        "started": 1586715571808,
        "metrics": {
            "partitionCount": 1,
            "outputByteSize": 2380339,
            "inputFileCount": -1,
            "inputByteSize": 2381007,
            "outputRecordCount": 24340,
            "outputFileCount": 1,
            "inputRecordCount": 24340
        },
        "completed": 1586715582735,
        "created": 1586715571217,
        "createdClient": "acp_foundation_push",
        "createdUser": "sensei_exp_attributionai@AdobeID",
        "updatedUser": "acp_foundation_dataTracker@AdobeID",
        "updated": 1586715583582,
        "version": "1.0.5"
    }
}
```

## バッチIDを使用した次のAPI呼び出しを取得する {#retrieve-the-next-api-call-with-your-batch-id}

バッチIDを取得すると、に新しいGETリクエストを作成でき `/batches`ます。 リクエストは、次のAPIリクエストとして使用されるリンクを返します。

**API形式**

```http
GET batches/{BATCH_ID}/files
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 前の手順で取得したバッチIDは、バッチID [を取得します](#retrieve-your-batch-id)。 |

**リクエスト**

独自のバッチIDを使用して、次のリクエストを行います。

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/batches/035e2520-5e69-11ea-b624-51evfeba55d1/files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、 `_links` オブジェクトを含むペイロードを返します。 オブジェクト内 `_links` には、新しいAPI呼び出し `href` を値として含むが、 この値をコピーして、次の手順に進みます。

```json
{
    "data": [
        {
            "dataSetFileId": "035e2520-5e69-11ea-b624-51ecfeba55d0-1",
            "dataSetViewId": "5e3b2fe3fe4b9f18a8b7a3db",
            "version": "1.0.0",
            "created": "1583361894479",
            "updated": "1583361894479",
            "isValid": false,
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/export/files/035e2520-5e69-11ea-b624-51ecfeba55d0-1"
                }
            }
        }
    ],
    "_page": {
        "limit": 100,
        "count": 1
    }
}
```

## ファイルの取得 {#retrieving-your-files}

前の手順で取得した `href` 値をAPI呼び出しとして使用し、新しいGETリクエストを作成してファイルディレクトリを取得します。

**API形式**

```http
GET files/{DATASETFILE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASETFILE_ID}` | dataSetFile IDは、 `href` 前の手順の [値で返されます](#retrieve-the-next-api-call-with-your-batch-id)。 また、オブジェクトタイプの `data` 配列でもアクセスでき `dataSetFileId`ます。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io:443/data/foundation/export/files/035e2520-5e69-11ea-b624-51ecfeba55d0-1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答には、1つのエントリを持つデータ配列、またはそのディレクトリに属するファイルのリストが含まれます。 次の例は、ファイルのリストを示しており、読みやすくまとめられています。 このシナリオでは、各ファイルにアクセスするには、各ファイルのURLに従う必要があります。

```json
{
    "data": [
        {
            "name": "part-00000-tid-7597930103898538622-a25f1890-efa9-40eb-a2cb-1b378e93d582-528-1-c000.snappy.parquet",
            "length": "16214531",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/export/files/035e2520-5e69-11ea-b624-51ecfeba55d0-1?path=part-00000-tid-7597930103898538622-a25f1890-efa9-40eb-a2cb-1b378e93d582-528-1-c000.snappy.parquet"
                }
            }
        },
        {
            "name": "...",
            "length": "16235375",
            "_links": {
                "self": {
                    "href": "..."
                }
            }
        }
    ],
    "_page": {
        "limit": 100,
        "count": 100
    },
    "_links": {
        "next": {
            "href": "..."
        },
        "page": {
            "href": "...",
            "templated": true
        }
    }
}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `_links.self.href` | ディレクトリ内のファイルのダウンロードに使用するGET要求URLです。 |


アレイ内の任意のファイルオブジェクトの `href` 値をコピーし、 `data` 次の手順に進みます。

## ファイルデータのダウンロード

ファイルデータをダウンロードするには、前の手順でコピーした `"href"` 値にGETリクエストを行い、ファイル [を取得します](#retrieving-your-files)。

>[!NOTE] この要求をコマンドラインで直接行う場合、要求ヘッダーの後に出力を追加するよう求められる場合があります。 次のリクエストの例は、を使用してい `--output {FILENAME.FILETYPE}`ます。

**API形式**

```http
GET files/{DATASETFILE_ID}?path={FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASETFILE_ID}` | dataSetFile IDは、 `href` 前の手順の [値で返されます](#retrieve-the-next-api-call-with-your-batch-id)。 |
| `{FILE_NAME}` | ファイルの名前。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io:443/data/foundation/export/files/035e2520-5e69-11ea-b624-51ecfeba55d0-1?path=part-00000-tid-7597930103898538622-a25f1890-efa9-40eb-a2cb-1b378e93d582-528-1-c000.snappy.parquet' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -O 'filename.parquet'
```

>[!TIP] GETリクエストを行う前に、ファイルを保存するディレクトリまたはフォルダーが正しいことを確認してください。

**応答**

応答によって、現在のディレクトリで要求したファイルがダウンロードされます。 この例では、ファイル名は「filename.parket」です。

![ターミナル](../images/download-scores/response.png)

## 顧客AIで設定されたセグメントのダウンロード {#segment}

スコアデータをダウンロードする別の方法は、オーディエンスをデータセットにエクスポートすることです。 セグメント化ジョブが正常に完了したら( `status` 属性の値は「SUCCEEDED」です)、オーディエンスをデータセットにエクスポートし、データセットにアクセスして処理できます。 セグメント化について詳しくは、 [セグメント化の概要を参照してください](../../../segmentation/home.md)。

>[!IMPORTANT] この書き出し方法を利用するには、データセットに対してリアルタイム顧客プロファイルを有効にする必要があります。

セグメント評価ガイドの「 [セグメントの](../../../segmentation/tutorials/evaluate-a-segment.md) エクスポート」の節では、オーディエンスデータセットのエクスポートに必要な手順について説明しています。 このガイドは、次の例の概要と例を示しています。

- **ターゲットデータセットの作成：** オーディエンスメンバーを格納するデータセットを作成します。
- **データセット内でのオーディエンスプロファイルの生成：** セグメントジョブの結果に基づいて、XDM個々のプロファイルにデータセットを設定します。
- **書き出しの進行状況の監視：** 書き出し処理の現在の進行状況を確認します。
- **オーディエンスデータの読み取り：** オーディエンスのメンバーを表すXDM個々のプロファイルを取得します。

## 次の手順

このドキュメントでは、顧客のAIスコアをダウンロードする際に必要な手順を説明しています。 提供される他の [インテリジェントサービス](../../home.md) 、ガイドを引き続き参照できるようになりました。
