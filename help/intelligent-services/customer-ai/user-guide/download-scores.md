---
keywords: Experience Platform;download scores;customer ai;popular topics
solution: Experience Platform
title: 顧客AIでのスコアのダウンロード
topic: Downloading scores
translation-type: tm+mt
source-git-commit: 1cf1e9c814601bdd4c7198463593be78452004dc

---


# 顧客AIでのスコアのダウンロード

このドキュメントは、顧客AIのスコアをダウンロードする際のガイドとして機能します。

## はじめに

顧客AIでは、パーケファイル形式でスコアをダウンロードできます。 このチュートリアルでは、はじめにのガイドの「お客様のAIスコアのダウンロード」の節を読み終えてい [る必要があ](../getting-started.md) ります。

さらに、顧客AIのスコアにアクセスするには、正常に実行されたステータスを持つサービスインスタンスを使用できる必要があります。 新しいサービスインスタンスを作成するには、「顧客AI [インスタンスの設定」にアクセスしま](./configure.md)す。 サービスインスタンスを最近作成し、まだトレーニングとスコアリングを行っている場合は、実行が終了するまで24時間お待ちください。

現在、顧客AIスコアをダウンロードする方法は2つあります。

1. 個々のレベルでスコアをダウンロードしたい場合や、リアルタイム顧客プロファイルが有効になっていない場合は、開始を行い、データセットIDを [探してください](#dataset-id)。
2. プロファイルを有効にしていて、顧客AIを使用して設定したセグメントをダウンロードする場合は、顧客AIを使用して設定したセ [グメントをダウンロードするように移動しま](#segment)す。

## Find your dataset ID {#dataset-id}

顧客AIインサイトのサービスインスタンス内で、右上のナビゲ *ーションの* 「その他のアクション」ドロップダウンをクリックし、「アクセススコア」を **[!UICONTROL 選択します]**。

![その他のアクション](../images/insights/more-actions.png)

新しいダイアログが表示され、ダウンロードスコアのドキュメントへのリンクと、現在のインスタンスのデータセットIDが含まれます。 データセットIDをクリップボードにコピーし、次の手順に進みます。

![データセットID](../images/download-scores/access-scores.png)

## バッチIDの取得 {#retrieve-your-batch-id}

前の手順のデータセットIDを使用して、バッチIDを取得するには、Catalog APIを呼び出す必要があります。 追加のクエリパラメーターは、組織に属するバッチのリストではなく、単一のバッチを返すために、このAPI呼び出しに使用されます。 使用可能なクエリパラメータのタイプについて詳しくは、カタログパラメータを使用したカタログデータのフ [ィルタリングに関するガイドを参照してくださ](../../../catalog/api/filter-data.md)い。

**API形式**

```http
GET /batches?&dataSet={DATASET_ID}&orderBy=desc:created&limit=1
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | 「アクセススコア」ダイアログで使用できるデータセットID。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/catalog/batches?dataSet=5cd9146b31dae914b75f654f&orderBy=desc:created&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、スコアバッチIDオブジェクトを含むペイロードを返します。 In this example, the object is `5e602f67c2f39715a87f46b1`.

スコアバッチIDオブジェクト内には配列が含ま `relatedObjects` れます。 この配列には2つのオブジェクトが含まれています。 最初のオブジェクトの値 `type` は「batch」で、IDも含まれています。 次の応答の例では、バッチIDはです `035e2520-5e69-11ea-b624-51evfeba55d1`。 次のAPI呼び出しで使用するバッチIDをコピーします。

```json
{   
    "5e602f67c2f39715a87f46b1": {
        "imsOrg": "{IMS_ORG}",
        "relatedObjects": [
            {
                "id": "5c01a91863540e14cd3d0432",
                "type": "dataSet"
            },
            {
                "id": "035e2520-5e69-11ea-b624-51evfeba55d1",
                "type": "batch"
            }
        ],
        "status": "success",
        "metrics": {
            "recordsRead": 46721830,
            "recordsWritten": 46721830,
            "avgNumExecutorsPerTask": 33,
            "startTime": 1583362385336,
            "inputSizeInKb": 10242034,
            "endTime": 1583363197517
        },
        "errors": [],
        "created": 1550791457173,
        "createdClient": "{CLIENT_CREATED}",
        "createdUser": "{CREATED_BY}",
        "updatedUser": "{CREATED_BY}",
        "updated": 1550792060301,
        "version": "1.0.116"
    }
}
```

## バッチIDを使用して次のAPI呼び出しを取得する {#retrieve-the-next-api-call-with-your-batch-id}

バッチIDを取得すると、に新しいGETリクエストを作成できます `/batches`。 リクエストは、次のAPIリクエストとして使用されるリンクを返します。

**API形式**

```http
GET batches/{BATCH_ID}/files
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 前の手順で取得したバッチID [です](#retrieve-your-batch-id)。 |

**リクエスト**

独自のバッチIDを使用して、次のリクエストを作成します。

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/batches/035e2520-5e69-11ea-b624-51evfeba55d1/files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、オブジェクトを含むペイロードを返 `_links` します。 オブジェクト `_links` 内には、新し `href` いAPI呼び出しを値として含むが、 次の手順に進むには、この値をコピーします。

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

前の手順で `href` 取得した値をAPI呼び出しとして使用し、新しいGETリクエストを作成して、ファイルディレクトリを取得します。

**API形式**

```http
GET files/{DATASETFILE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASETFILE_ID}` | dataSetFile IDは、前の手順の値 `href` で返さ [れます](#retrieve-the-next-api-call-with-your-batch-id)。 また、オブジェクトタイプの下の配 `data` 列からもアクセスできま `dataSetFileId`す。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io:443/data/foundation/export/files/035e2520-5e69-11ea-b624-51ecfeba55d0-1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答には、単一のエントリを持つデータ配列、またはそのディレクトリに属するファイルのリストが含まれます。 次の例は、ファイルのリストを示し、読みやすくするために要約されています。 このシナリオでは、各ファイルのURLをたどって、ファイルにアクセスする必要があります。

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


配列内の `href` 任意のファイルオブジェクトの値 `data` をコピーし、次の手順に進みます。

## ファイルデータのダウンロード

ファイルデータをダウンロードするには、前の手順でコピーし `"href"` た値に対してGETリクエストを [実行します](#retrieving-your-files)。

>[!NOTE] この要求をコマンドラインで直接行う場合は、要求ヘッダーの後に出力を追加するように求められる場合があります。 次のリクエストの例では、を使用していま `--output {FILENAME.FILETYPE}`す。

**API形式**

```http
GET files/{DATASETFILE_ID}?path={FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASETFILE_ID}` | dataSetFile IDは、前の手順の値 `href` で返さ [れます](#retrieve-the-next-api-call-with-your-batch-id)。 |
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

応答により、現在のディレクトリで要求したファイルがダウンロードされます。 この例では、ファイル名は「filename.parket」です。

![ターミナル](../images/download-scores/response.png)

## 顧客AIで設定したセグメントのダウンロード {#segment}

スコアデータをダウンロードする別の方法は、データセットにオーディエンスを書き出すことです。 セグメント化ジョブが正常に完了したら（属性の値は「SUCCEEDED」）、オーディエンスをデータセットにエクスポートし、そこでアクセスし、処理を行うことができます。 `status` セグメント化について詳しくは、セグメントの概要を参 [照してくださ](../../../segmentation/home.md)い。

>[!IMPORTANT] この書き出し方法を利用するには、データセットに対してリアルタイム顧客プロファイルを有効にする必要があります。

セグメント [評価ガイドの「セグメントの書き出し](../../../segmentation/tutorials/evaluate-a-segment.md) 」の節では、オーディエンスデータセットの書き出しに必要な手順について説明します。 このガイドでは、次の例の概要を説明しています。

- **ターゲットデータセットの作成：** データセットを作成して、オーディエンスメンバーを保持します。
- **オーディエンスセットでのプロファイルの生成：** セグメントジョブの結果に基づいて、プロファイルセットにXDM個々のジョブを設定します。
- **書き出しの進行状況の監視：** 書き出し処理の現在の進行状況を確認します。
- **読み取りオーディエンスデータ：** 表示されるXDM個々のプロファイルを取得します。オーディエンスのメンバー。

## 次の手順

このドキュメントでは、顧客AIスコアのダウンロードに必要な手順を説明しています。 提供される他のインテリジェントサービス [とガイドを](../../home.md) 、引き続き参照できます。
