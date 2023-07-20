---
keywords: Experience Platform、ダウンロードスコア、顧客 AI、人気の高いトピック、エクスポート、エクスポート、顧客 AI ダウンロード、顧客 AI スコア
solution: Experience Platform, Real-Time Customer Data Platform
feature: Customer AI
title: 顧客 AI でのスコアのダウンロード
description: 顧客 AI を使用すると、Parquet ファイル形式でスコアをダウンロードできます。
exl-id: 08f05565-3fd4-4089-9c41-32467f0be751
source-git-commit: 07a110f6d293abff38804b939014e28f308e3b30
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 81%

---

# 顧客 AI でのスコアのダウンロード

このドキュメントは、顧客 AI のスコアをダウンロードする際のガイドとして提供されています。

## はじめに

顧客 AI を使用すると、Parquet ファイル形式でスコアをダウンロードできます。 このチュートリアルでは、[開始ガイド](../getting-started.md)の顧客 AI スコアのダウンロードに関するセクションを読み終えていることを前提としています。

さらに、顧客 AI のスコアにアクセスするには、正常に実行されたステータスを持つサービスインスタンスが必要です。新しいサービスインスタンスを作成するには、次にアクセスします。 [顧客 AI インスタンスの設定](./configure.md). サービスインスタンスを作成したばかりで、まだトレーニングとスコア測定を行っている場合は、実行が終了するまで 24 時間お待ちください。

現在は、2 つの方法で顧客 AI スコアをダウンロードできます。

1. 個々のレベルでスコアをダウンロードする場合や、リアルタイム顧客プロファイルが有効になっていない場合は、まず、 [データセット ID の検索](#dataset-id).
2. プロファイルが有効で、顧客 AI を使用して設定したセグメントをダウンロードする場合は、[こちら](#segment)の手順を参照してください。

## データセット ID を見つける {#dataset-id}

顧客 AI インサイトのサービスインスタンス内で、右上のナビゲーションの「*その他のアクション*」ドロップダウンをクリックし、「**[!UICONTROL アクセススコア]**」を選択します。

![その他のアクション](../images/insights/more-actions.png)

新しいダイアログが表示され、スコアのダウンロードに関するドキュメントへのリンクと、現在のインスタンスのデータセット ID が示されます。データセット ID をクリップボードにコピーして、次の手順に進みます。

![データセット ID](../images/download-scores/access-scores.png)

## バッチ ID を取得する {#retrieve-your-batch-id}

前の手順で取得したデータセット ID を使用してバッチ ID を取得するには、Catalog API への呼び出しを実行する必要があります。追加のクエリパラメーターは、組織に属するバッチのリストではなく、最新の成功したバッチを返すために、この API 呼び出しに使用されます。 追加のバッチを返すには、limit クエリーパラメーターの数を、返す数に増やします。 使用可能なクエリパラメーターの種類について詳しくは、[クエリパラメーターを使用したカタログデータのフィルタリング](../../../catalog/api/filter-data.md)に関するガイドを参照してください。

**API 形式**

```http
GET /batches?&dataSet={DATASET_ID}&createdClient=acp_foundation_push&status=success&orderBy=desc:created&limit=1
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | アクセススコアダイアログで使用できるデータセット ID です。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/catalog/batches?dataSet=5cd9146b31dae914b75f654f&createdClient=acp_foundation_push&status=success&orderBy=desc:created&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、バッチ ID オブジェクトを含むペイロードを返します。 この例では、返されるオブジェクトのキー値はバッチ ID です `01E5QSWCAASFQ054FNBKYV6TIQ`. バッチ ID をコピーして、次の API 呼び出しで使用します。

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

## バッチ ID を使用して次の API 呼び出しを取得する {#retrieve-the-next-api-call-with-your-batch-id}

バッチ ID を取得したら、`/batches` に対して新しい GET リクエストを実行できます。リクエストを実行すると、次の API リクエストとして使用するリンクが返されます。

**API 形式**

```http
GET batches/{BATCH_ID}/files
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BATCH_ID}` | 前の手順（[バッチ ID の取得](#retrieve-your-batch-id)）で取得したバッチ ID です。 |

**リクエスト**

独自のバッチ ID を使用して、次のリクエストを実行します。

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/batches/035e2520-5e69-11ea-b624-51evfeba55d1/files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、`_links` オブジェクトを含むペイロードが返されます。`_links` オブジェクト内には、新しい API 呼び出しが値として付加された `href` があります。この値をコピーして、次の手順に進みます。

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

## ファイルを取得する {#retrieving-your-files}

前の手順で取得した `href` 値を API 呼び出しとして使用して、新しい GET リクエストを実行し、ファイルディレクトリを取得します。

**API 形式**

```http
GET files/{DATASETFILE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASETFILE_ID}` | dataSetFile ID は、[前の手順](#retrieve-the-next-api-call-with-your-batch-id)で取得した `href` 値にあります。また、オブジェクトタイプ `dataSetFileId` 下の `data` 配列からもアクセスできます。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io:443/data/foundation/export/files/035e2520-5e69-11ea-b624-51ecfeba55d0-1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

レスポンスには、データ配列（単一のエントリが存在する可能性があります）、またはそのディレクトリに属するファイルのリストが含まれます。次の例にはファイルのリストが含まれています。また、読みやすくするために簡略化されています。このシナリオの場合、ファイルにアクセスするには各ファイルの URL をたどる必要があります。

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
| `_links.self.href` | ディレクトリ内のファイルのダウンロードに使用する GET 要求の URL です。 |


`data` 配列内のファイルオブジェクトの `href` 値をコピーして、次の手順に進みます。

## ファイルデータをダウンロードする

ファイルデータをダウンロードするには、前の手順（[ファイルの取得](#retrieving-your-files)）でコピーした `"href"` 値に対して GET リクエストを実行します。

>[!NOTE]
>
>このリクエストをコマンドラインで直接おこなう場合、要求ヘッダーの後に出力を追加するように求められる場合があります。次のリクエストの例では `--output {FILENAME.FILETYPE}` を使用しています。

**API 形式**

```http
GET files/{DATASETFILE_ID}?path={FILE_NAME}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASETFILE_ID}` | dataSetFile ID は、[前の手順](#retrieve-the-next-api-call-with-your-batch-id)で取得した `href` 値にあります。 |
| `{FILE_NAME}` | ファイルの名前です。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io:443/data/foundation/export/files/035e2520-5e69-11ea-b624-51ecfeba55d0-1?path=part-00000-tid-7597930103898538622-a25f1890-efa9-40eb-a2cb-1b378e93d582-528-1-c000.snappy.parquet' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -O 'filename.parquet'
```

>[!TIP]
>
>GET リクエストを実行する前に、ファイルを保存するディレクトリまたはフォルダーが正しいことを確認してください。

**応答**

現在のディレクトリにある、要求したファイルがダウンロードされます。この例では、ファイル名は「filename.parket」です。

![端末](../images/download-scores/response.png)

## 顧客 AI で設定したセグメントをダウンロードする {#segment}

オーディエンスをデータセットに書き出すことで、スコアデータをダウンロードすることもできます。セグメントジョブが正常に完了したら（`status` 属性の値は「SUCCEEDED」）、オーディエンスをデータセットにエクスポートして、オーディエンスにアクセスし、処理をおこなうことができます。セグメント化について詳しくは、[セグメントの概要](../../../segmentation/home.md)を参照してください。

>[!IMPORTANT]
>
>この書き出し方法を利用するには、データセットに対してリアルタイム顧客プロファイルを有効にする必要があります。

セグメント評価ガイドの[セグメントの書き出し](../../../segmentation/tutorials/evaluate-a-segment.md)に関する節では、オーディエンスデータセットの書き出しに必要な手順について説明しています。このガイドでは、次の操作の説明と例が示されています。

- **ターゲットデータセットの作成：**&#x200B;オーディエンスメンバーを保持するデータセットを作成する。
- **データセットでのオーディエンスプロファイルの生成：**&#x200B;セグメントジョブの結果に基づいて、データセットに XDM の個別プロファイルを入力する。
- **書き出しの進行状況の監視：**&#x200B;書き出し処理の現在の進行状況を確認する。
- **オーディエンスデータの読み取り：**&#x200B;オーディエンスのメンバーを表す XDM の個別プロファイルを取得する。

## 次の手順

このドキュメントでは、顧客 AI スコアのダウンロードに必要な手順について説明しました。これで、提供されているその他の[インテリジェントサービス](../../home.md)とガイドを参照できます。
