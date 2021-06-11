---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；プレビュー；予測；プレビューと推定；推定；推定とプレビュー；api;API;
solution: Experience Platform
title: プレビューと推定APIエンドポイント
topic-legacy: developer guide
description: セグメント定義の作成時に、Adobe Experience Platform内の推定ツールとプレビューツールを使用して概要レベルの情報を表示し、期待されるオーディエンスを確実に特定することができます。
exl-id: 2c204f29-825f-4a5e-a7f6-40fc69263614
source-git-commit: a5cc688357e4750dee73baf3fc9af02a9f2e49e3
workflow-type: tm+mt
source-wordcount: '978'
ht-degree: 19%

---

# プレビューとエンドポイントの推定

セグメント定義の作成時に、Adobe Experience Platformの推定ツールとプレビューツールを使用して概要レベルの情報を表示し、期待するオーディエンスを確実に特定することができます。

* **プレビューは、セグメント定義に適格なプロファイルのページ分割リストを表示するので、結果を予想と比較できます。**

* **** 推定値は、予測されるオーディエンスサイズ、信頼区間、誤差の標準偏差など、セグメント定義の統計情報を提供します。

>[!NOTE]
>
>特定の名前空間内のプロファイルフラグメントと結合されたプロファイルの合計数、またはプロファイルデータストア全体など、リアルタイム顧客プロファイルデータに関連する類似の指標にアクセスするには、『Profile API開発者ガイド』の「[profile preview (sample status) endpoint guide](../../profile/api/preview-sample-status.md)」を参照してください。

## 概要

このガイドで使用する エンドポイントは、[!DNL Adobe Experience Platform Segmentation Service]API の一部です。続行する前に、『[はじめに](./getting-started.md)』を参照して、必要なヘッダーやサンプルAPI呼び出しを含むAPIの呼び出しを正しく実行するために知っておく必要がある重要な情報を確認してください。

## 推定の生成方法

プロファイルストアへのレコードの取り込みで、プロファイルの合計数が5%を超えるまたは減少すると、サンプリングジョブがトリガーされ、カウントが更新されます。 データサンプリングをトリガーする方法は、取り込み方法によって異なります。

* **バッチ取得：** バッチ取得の場合、15分以内にプロファイルストアにバッチを正常に取り込みます。5%の増減しきい値に達すると、ジョブが実行されてカウントが更新されます。
* **ストリーミングの取り込み：** ストリーミングデータワークフローの場合、5%増減のしきい値に達したかどうかを判断するために、1時間ごとにチェックがおこなわれます。ある場合は、ジョブが自動的にトリガーされ、カウントが更新されます。

スキャンのサンプルサイズは、プロファイルストア内のエンティティの全体数によって異なります。 これらのサンプルサイズを次の表に示します。

| プロファイルストア内のエンティティ数 | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

>[!NOTE]
>
>推定の実行には通常、10～15秒かかります。最初は大まかな見積もりで、読み取るレコードが増えるにつれて精度が向上します。

## プレビューの新規作成 {#create-preview}

新しいプレビューを作成するには、`/preview` エンドポイントに POST リクエストを送信します。

>[!NOTE]
>
>プレビュージョブの作成時に、推定ジョブが自動的に作成されます。 これらの2つのジョブは同じIDを共有します。

**API 形式**

```http
POST /preview
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/preview \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
    {
        "predicateExpression": "xEvent.metrics.commerce.abandons.value > 0",
        "predicateType": "pql/text",
        "predicateModel": "_xdm.context.profile",
        "graphType": "none"
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `predicateExpression` | データのクエリに使用する PQL 式です。 |
| `predicateType` | `predicateExpression`の下のクエリ式の述語タイプ。 現在、このプロパティで使用できる値は`pql/text`のみです。 |
| `predicateModel` | プロファイルデータの基になる[!DNL Experience Data Model](XDM)スキーマクラスの名前。 |
| `graphType` | クラスターの取得元のグラフタイプ。 サポートされている値は`none`（IDステッチを実行しない）および`pdg`（個人のIDグラフに基づいてIDステッチを実行）です。 |

**応答**

成功時の応答は、HTTP ステータス 201（Created）と共に、新しく作成されたプレビューの詳細を返します。

```json
{
    "state": "NEW",
    "previewQueryId": "e890068b-f5ca-4a8f-a6b5-af87ff0caac3",
    "previewQueryStatus": "NEW",
    "previewId": "MDphcHAtMzJiZTAzMjgtM2YzMS00YjY0LThkODQtYWNkMGM0ZmJkYWQzOmU4OTAwNjhiLWY1Y2EtNGE4Zi1hNmI1LWFmODdmZjBjYWFjMzow",
    "previewExecutionId": 0
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `state` | プレビュージョブの現在の状態です。最初に作成した場合は、「NEW」状態になります。 その後、処理が完了するまで「RUNNING」状態になり、完了した時点で「RESULT_READY」または「FAILED」になります。 |
| `previewId` | 次の節で説明するように、推定またはプレビューを表示する際に参照目的で使用されるプレビュージョブのID。 |

## 特定のプレビューの結果を取得します。{#get-preview}

特定のプレビューに関する詳細な情報を取得するには、`/preview`エンドポイントにGETリクエストを送信し、リクエストパスにプレビューIDを指定します。

**API 形式**

```http
GET /preview/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 取得するプレビューの`previewId`値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/preview/MDphcHAtMzJiZTAzMjgtM2YzMS00YjY0LThkODQtYWNkMGM0ZmJkYWQzOmU4OTAwNjhiLWY1Y2EtNGE4Zi1hNmI1LWFmODdmZjBjYWFjMzow \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功時の応答は、HTTP ステータス 200 と共に、指定されたプレビューに関する詳細な情報を返します。

```json
{
   "results": [{
        "XID_ADOBE-MARKETING-CLOUD-ID-1": {
            "_href": "https://platform.adobe.io/data/core/ups/models/profile/XID_ADOBE-MARKETING-CLOUD-ID-1",
            "endCustomerIds": {
                "XID_COOKIE_ID_1": {
                    "_href": "https://platform.adobe.io/data/core/ups/models/profile/XID_COOKIE_ID_1"
                },
                "XID_PROFILE_ID_1": {
                    "_href": "https://platform.adobe.io/data/core/ups/models/profile/XID_PROFILE_ID_1"
                }
            }
        }
    },
    {
        "XID_COOKIE-ID-2": {
            "_href": "https://platform.adobe.io/data/core/ups/models/profile/XID_COOKIE-ID-2",
            "endCustomerIds": {
                "XID_COOKIE_ID_2-1": {
                    "_href": "https://platform.adobe.io/data/core/ups/models/profile/XID_COOKIE_ID_2-1"

                },
                "XID_PROFILE_ID_2": {
                    "_href": "https://platform.adobe.io/data/core/ups/models/profile/XID_PROFILE_ID_2"
                }
            }
        },
        "XID_ADOBE-MARKETING-CLOUD-ID-3": {
            "_href": "https://platform.adobe.io/data/core/ups/models/profile/XID_ADOBE-MARKETING-CLOUD-ID-1000"
        }
    }],
    "state": "RESULT_READY",
    "links": {
        "_self": "https://platform.adobe.io/data/core/ups/preview?expression=<expr-1>&limit=1000",
        "next": "",
        "prev": ""
    },
    "page": {
        "offset": 0,
        "size": 3
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `results` | エンティティIDのリストと関連ID。 提供されたリンクは、[プロファイルアクセスAPIエンドポイント](../../profile/api/entities.md)を使用して、指定されたエンティティを検索するために使用できます。 |

## 特定の見積ジョブの結果の取得 {#get-estimate}

プレビュージョブを作成したら、 `/estimate`エンドポイントへのGETリクエストのパスでその`previewId`を使用して、予測されるオーディエンスサイズ、信頼区間、誤差の標準偏差など、セグメント定義の統計情報を表示できます。

**API 形式**

```http
GET /estimate/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 推定ジョブは、プレビュージョブが作成された場合にのみトリガーされ、2つのジョブは、参照用に同じID値を共有します。 特に、プレビュージョブが作成されたときに返される`previewId`値です。 |

**リクエスト**

次のリクエストは、特定の見積ジョブの結果を取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/estimate/MDoyOjRhNDVlODUzLWFjOTEtNGJiNy1hNDI2LTE1MDkzN2I2YWY1Yzo0Mg \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、見積ジョブの詳細を含む HTTP ステータス 200 を返します。

```json
{
    "estimatedSize": 4275,
    "numRowsToRead": 4275,
    "estimatedNamespaceDistribution": [
        {
            "namespaceId": "4",
            "profilesMatchedSoFar": 35
        },
        {
            "namespaceId": "6",
            "profilesMatchedSoFar": 4275
        }
    ],
    "state": "RESULT_READY",
    "profilesReadSoFar": 4275,
    "standardError": 0,
    "error": {
        "description": "",
        "traceback": ""
    },
    "profilesMatchedSoFar": 4275,
    "totalRows": 4275,
    "confidenceInterval": "95%",
    "_links": {
        "preview": "https://platform.adobe.io/data/core/ups/preview/app-32be0328-3f31-4b64-8d84-acd0c4fbdad3/execution/0?previewQueryId=e890068b-f5ca-4a8f-a6b5-af87ff0caac3"
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `estimatedNamespaceDistribution` | ID名前空間で分類された、セグメント内のプロファイル数を示すオブジェクトの配列。 1つのプロファイルが複数の名前空間に関連付けられる可能性があるので、名前空間別のプロファイルの合計数（各名前空間に表示される値を加算）は、プロファイル数指標より多くなる場合があります。 例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。 |
| `state` | プレビュージョブの現在の状態です。処理が完了するまで、状態は「RUNNING」になり、完了した時点で、「RESULT_READY」または「FAILED」になります。 |
| `_links.preview` | `state`が「RESULT_READY」の場合、このフィールドには推定を表示するURLが表示されます。 |

## 次の手順

このガイドを読んだ後、Segmentation APIを使用したプレビューと推定の操作方法をより深く理解する必要があります。 特定の名前空間内のプロファイルフラグメントと結合されたプロファイルの合計数、またはプロファイルデータストア全体など、リアルタイム顧客プロファイルデータに関連する指標にアクセスする方法については、『[プロファイルプレビュー(`/previewsamplestatus`)エンドポイントガイド](../../profile/api/preview-sample-status.md)』を参照してください。
