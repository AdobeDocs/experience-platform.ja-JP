---
solution: Experience Platform
title: API エンドポイントのプレビューと推定
description: セグメントの定義が作成されたら、Adobe Experience Platformの見積もりツールとプレビューツールを使用して概要レベルの情報を表示し、想定されるオーディエンスを確実に特定できます。
role: Developer
exl-id: 2c204f29-825f-4a5e-a7f6-40fc69263614
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 18%

---

# エンドポイントのプレビューと推定

セグメント定義を作成する際は、Adobe Experience Platformの見積もりツールとプレビューツールを使用して、概要レベルの情報を確認し、想定するオーディエンスを確実に分離することができます。

* **プレビュー**&#x200B;には、セグメント定義の対象となるプロファイルのページ化されたリストが表示され、結果を期待するものと比較できます。

* **推定**&#x200B;は、予測オーディエンス サイズ、信頼区間、エラー標準偏差など、セグメント定義に関する統計情報を提供します。

>[!NOTE]
>
>特定の名前空間内のプロファイルフラグメントと結合プロファイルの合計数やプロファイルデータストア全体など、リアルタイム顧客プロファイルデータに関連する類似の指標にアクセスするには、「[&#x200B; プロファイルのプレビュー（サンプルステータスのプレビュー）」エンドポイントガイド &#x200B;](../../profile/api/preview-sample-status.md)、Profile API開発者ガイドの一部を参照してください。

## はじめに

このガイドで使用されているエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 続行する前に、必須ヘッダーやサンプル API呼び出しの読み取り方法など、APIへの呼び出しを正常に行うために知っておく必要がある重要な情報については、[入門ガイド &#x200B;](./getting-started.md)を確認してください。

## 推定の生成方法

プロファイルストアにレコードを取り込むと、合計プロファイル数が3%以上増加または減少すると、サンプリングジョブがトリガーされて数が更新されます。 データサンプリングのトリガー方法は、取り込み方法によって異なります。

* **バッチ取り込み：** バッチ取り込みの場合、プロファイルストアにバッチを正常に取り込んでから15分以内に、3%の増減しきい値を満たした場合、カウントを更新するジョブが実行されます。
* **ストリーミング取り込み：** ストリーミングデータワークフローの場合、3%の増減しきい値が満たされているかどうかを判断するために、1時間ごとにチェックが行われます。 が含まれている場合、ジョブは自動的にトリガーされ、カウントが更新されます。

スキャンのサンプルサイズは、プロファイルストア内のエンティティの総数によって異なります。 これらのサンプルサイズを次の表に示します。

| プロファイルストアのエンティティ | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

>[!NOTE]
>
>一般的に、見積もりは10～15秒かかります。最初に大まかな見積もりをおこない、より多くのレコードを読み取るにつれて調整します。

## プレビューの新規作成 {#create-preview}

新しいプレビューを作成するには、`/preview` エンドポイントに POST リクエストを送信します。

>[!NOTE]
>
>プレビュージョブが作成されると、見積もりジョブが自動的に作成されます。 これらの2つのジョブは同じIDを共有します。

**API 形式**

```http
POST /preview
```

**リクエスト**

+++ プレビューを作成するためのサンプルリクエスト。

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
| `predicateType` | `predicateExpression`の下のクエリ式の述語タイプ。 現在、このプロパティで許可されている値は`pql/text`のみです。 |
| `predicateModel` | プロファイルデータの基となる[!DNL Experience Data Model] （XDM） スキーマクラスの名前。 |
| `graphType` | クラスターを取得するグラフタイプ。 サポートされている値は`none` （ID ステッチを実行しません）と`pdg` （プライベート ID グラフに基づいてID ステッチを実行します）です。 |

+++

**応答**

成功時の応答は、HTTP ステータス 201（Created）と共に、新しく作成されたプレビューの詳細を返します。

+++ プレビューの作成時のサンプル応答。

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
| `state` | プレビュージョブの現在の状態です。最初に作成されると、「新しい」状態になります。 その後、処理が完了するまで「RUNNING」状態になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `previewId` | 次の節で説明するように、見積もりやプレビューを表示する際に参照に使用するプレビュージョブのID。 |

+++

## 特定のプレビューの結果の取得 {#get-preview}

`/preview` エンドポイントに対してGET リクエストを行い、リクエストパスにプレビューIDを指定することで、特定のプレビューに関する詳細な情報を取得できます。

**API 形式**

```http
GET /preview/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 取得するプレビューの`previewId`値。 |

**リクエスト**

+++ プレビューを取得するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/preview/MDphcHAtMzJiZTAzMjgtM2YzMS00YjY0LThkODQtYWNkMGM0ZmJkYWQzOmU4OTAwNjhiLWY1Y2EtNGE4Zi1hNmI1LWFmODdmZjBjYWFjMzow \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

+++ プレビューを取得する際のサンプル応答。

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
| `results` | エンティティ IDとその関連IDのリスト。 指定されたリンクは、[&#x200B; プロファイルアクセス API エンドポイント &#x200B;](../../profile/api/entities.md)を使用して、指定されたエンティティを検索するために使用できます。 |

+++

## 特定の見積ジョブの結果の取得 {#get-estimate}

プレビュージョブを作成したら、`previewId` エンドポイントへのGET リクエストのパスで`/estimate`を使用して、予測オーディエンスサイズ、信頼区間、エラー標準偏差など、セグメント定義に関する統計情報を表示できます。

**API 形式**

```http
GET /estimate/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 見積もりジョブは、プレビュージョブが作成されたときにのみトリガーされ、2つのジョブが検索目的で同じID値を共有します。 具体的には、プレビュージョブの作成時に返される`previewId`値です。 |

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

+++ 推定ジョブを取得する際の応答のサンプル。

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
| `estimatedNamespaceDistribution` | セグメント定義内のプロファイルの数をID名前空間ごとに示すオブジェクトの配列。 名前空間別のプロファイルの合計数（各名前空間に表示される値を合計）は、1つのプロファイルが複数の名前空間に関連付けられる可能性があるため、プロファイル数の指標よりも多くなる可能性があります。 例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。 |
| `state` | プレビュージョブの現在の状態です。処理が完了するまで、状態は「RUNNING」になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `_links.preview` | `state`が「RESULT_READY」の場合、このフィールドには見積もりを表示するURLが表示されます。 |

+++

## 次の手順

このガイドでは、Segmentation APIを使用してプレビューと見積もりを操作する方法について詳しく説明します。 特定の名前空間内のプロファイルフラグメントと結合プロファイルの合計数、プロファイルデータストア全体など、リアルタイム顧客プロファイルデータに関連する指標にアクセスする方法については、[&#x200B; プロファイルプレビュー（`/previewsamplestatus`） エンドポイントガイド &#x200B;](../../profile/api/preview-sample-status.md)を参照してください。
