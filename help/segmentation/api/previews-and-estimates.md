---
solution: Experience Platform
title: API エンドポイントのプレビューと見積もり
description: セグメント定義の開発時に、Adobe Experience Platform内の予測ツールとプレビューツールを使用して概要レベルの情報を表示すると、期待されるオーディエンスを確実に分離するのに役立ちます。
role: Developer
exl-id: 2c204f29-825f-4a5e-a7f6-40fc69263614
source-git-commit: a374d261e3b34b30869f1a9e8486d52f5bd658cb
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 18%

---

# エンドポイントのプレビューと見積もり

セグメント定義を作成する際には、Adobe Experience Platformの推定ツールとプレビューツールを使用して概要レベルの情報を表示し、期待するオーディエンスを確実に分離するのに役立ちます。

* **プレビュー** セグメント定義の選定中のプロファイルのページ分割されたリストを提供し、結果を期待するものと比較できるようにします。

* **予測** は、推定オーディエンスサイズ、信頼区間、エラー標準偏差など、セグメント定義に関する統計情報を提供します。

>[!NOTE]
>
>特定の名前空間内のプロファイルフラグメントと結合プロファイルの合計数や、プロファイルデータストア全体など、リアルタイム顧客プロファイルデータに関連する類似の指標にアクセスするには、プロファイル API 開発者ガイドの一部である [ プロファイルプレビュー（サンプルステータスのプレビュー）エンドポイントガイド ](../../profile/api/preview-sample-status.md) を参照してください。

## はじめに

このガイドで使用するエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] API の一部です。 続行する前に、[ はじめる前に ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください。

## 推定の生成方法

プロファイルストアへのレコードの取り込みが、合計プロファイル数を 3% 以上増加または減少させると、サンプリングジョブがトリガーされて数が更新されます。 データサンプリングをトリガーする方法は、取り込み方法によって異なります。

* **バッチ取り込み：** バッチ取り込みの場合、プロファイルストアにバッチを正常に取り込んでから 15 分以内に、3% の増減しきい値に達すると、ジョブを実行してカウントを更新します。
* **ストリーミング取得：** ストリーミングデータワークフローの場合は、3% の増加または減少しきい値に達したかどうかを判断するために、1 時間ごとにチェックが行われます。 カウントされる場合は、カウントを更新するジョブが自動的にトリガーされます。

スキャンのサンプルサイズは、プロファイルストア内のエンティティの合計数によって異なります。 これらのサンプルサイズを次の表に示します。

| プロファイルストアのエンティティ | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

>[!NOTE]
>
>見積もりは通常、大まかな見積もりから始まり、より多くのレコードが読み取られるにつれて絞り込まれるまで、実行に 10 ～ 15 秒かかります。

## プレビューの新規作成 {#create-preview}

新しいプレビューを作成するには、`/preview` エンドポイントに POST リクエストを送信します。

>[!NOTE]
>
>推定ジョブは、プレビュージョブの作成時に自動的に作成されます。 これら 2 つのジョブは同じ ID を共有します。

**API 形式**

```http
POST /preview
```

**リクエスト**

+++ プレビューを作成するリクエストのサンプル

```shell
curl -X POST https://platform.adobe.io/data/core/ups/preview \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `predicateType` | `predicateExpression` のクエリ式の述語タイプ。 現在、このプロパティに許可されている値は `pql/text` のみです。 |
| `predicateModel` | プロファイルデータの基となる [!DNL Experience Data Model] （XDM）スキーマクラスの名前。 |
| `graphType` | クラスターの取得元のグラフタイプ。 サポートされている値は、`none` （ID ステッチを実行しない）および `pdg` （プライベート ID グラフに基づいて ID ステッチを実行する）です。 |

+++

**応答**

成功時の応答は、HTTP ステータス 201（Created）と共に、新しく作成されたプレビューの詳細を返します。

+++ プレビュー作成時の応答例。

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
| `state` | プレビュージョブの現在の状態です。最初に作成されると、「新規」状態になります。 その後、処理が完了するまで「RUNNING」状態になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `previewId` | 次の節で説明するように、見積もりやプレビューを表示する際にルックアップ目的で使用されるプレビュージョブの ID。 |

+++

## 特定のプレビュー結果の取得 {#get-preview}

特定のプレビューに関する詳細な情報を取得するには、`/preview` エンドポイントにGET リクエストを実行し、リクエストパスでプレビュー ID を指定します。

**API 形式**

```http
GET /preview/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 取得するプレビューの `previewId` 値。 |

**リクエスト**

+++ プレビューを取得するリクエストのサンプル。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/preview/MDphcHAtMzJiZTAzMjgtM2YzMS00YjY0LThkODQtYWNkMGM0ZmJkYWQzOmU4OTAwNjhiLWY1Y2EtNGE4Zi1hNmI1LWFmODdmZjBjYWFjMzow \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

+++ プレビュー取得時の応答例。

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
| `results` | エンティティ ID のリストと関連する ID。 提供されるリンクは、[ プロファイルアクセス API エンドポイント ](../../profile/api/entities.md) を使用して、指定したエンティティを検索するために使用できます。 |

+++

## 特定の見積ジョブの結果の取得 {#get-estimate}

プレビュージョブを作成したら、`previewId` エンドポイントに対するGET リクエストのパスでその `/estimate` を使用して、予測オーディエンスサイズ、信頼区間、エラー標準偏差など、セグメント定義に関する統計情報を表示できます。

**API 形式**

```http
GET /estimate/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 推定ジョブは、プレビュージョブが作成され、2 つのジョブが参照目的で同じ ID 値を共有する場合にのみトリガーされます。 特に、これはプレビュージョブの作成時に返される `previewId` 値です。 |

**リクエスト**

次のリクエストは、特定の見積ジョブの結果を取得します。

+++ 推定ジョブを取得するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/estimate/MDoyOjRhNDVlODUzLWFjOTEtNGJiNy1hNDI2LTE1MDkzN2I2YWY1Yzo0Mg \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

成功した応答は、見積ジョブの詳細を含む HTTP ステータス 200 を返します。

+++ 推定ジョブを取得する際のサンプル応答。

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
| `estimatedNamespaceDistribution` | セグメント定義内のプロファイルの数を ID 名前空間別に分類して示すオブジェクトの配列。 1 つのプロファイルが複数の名前空間に関連付けられている可能性があるので、名前空間別のプロファイルの合計数（各名前空間に表示される値をまとめたもの）は、プロファイル数指標よりも多くなる場合があります。 例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。 |
| `state` | プレビュージョブの現在の状態です。処理が完了するまで状態は「RUNNING」になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `_links.preview` | `state` が「RESULT_READY」の場合、このフィールドには見積りを表示する URL が表示されます。 |

+++

## 次の手順

このガイドを読むことで、Segmentation API を使用したプレビューと見積りの操作方法について、理解を深めることができました。 特定の名前空間内のプロファイルフラグメントの合計数や結合されたプロファイル、プロファイルデータストア全体など、リアルタイム顧客プロファイルデータに関連する指標にアクセスする方法については、[ プロファイルプレビュー（`/previewsamplestatus`）エンドポイントガイド ](../../profile/api/preview-sample-status.md) を参照してください。
