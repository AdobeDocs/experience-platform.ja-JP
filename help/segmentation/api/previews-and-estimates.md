---
keywords: Experience Platform；ホーム；人気の高いトピック；セグメント化；セグメント化；セグメント化サービス；プレビュー；予測；プレビューと予測；予測とプレビュー;api;API;
solution: Experience Platform
title: プレビューと予測APIエンドポイント
topic-legacy: developer guide
description: セグメント定義が作成されると、Adobe Experience Platformの見積もりツールとプレビューツールを使用して、表示の概要レベルの情報を利用し、期待されるオーディエンスを確実に切り分けることができます。
exl-id: 2c204f29-825f-4a5e-a7f6-40fc69263614
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 18%

---

# エンドポイントのプレビューと予測

セグメント定義を作成する際に、Adobe Experience Platformの見積もりツールとプレビューツールを使用して、表示の概要レベルの情報を得ることで、期待するオーディエンスを確実に切り離すことができます。

* **プレビューは、セグメント定義に適格なプロファイルのページ分割リストを表示するので、結果を予想と比較できます。**

* **** 予測には、予測されるオーディエンスサイズ、信頼区間、誤差の標準偏差など、セグメント定義の統計情報が含まれます。

>[!NOTE]
>
>特定の名前空間またはプロファイルデータストア内のプロファイルフラグメントと結合プロファイルの合計数など、リアルタイム顧客プロファイルデータに関連する同様の指標にアクセスするには、『プロファイルAPI開発者ガイド』の「[プレビュープレビュー(プロファイルサンプルステータス)エンドポイントガイド](../../profile/api/preview-sample-status.md)」を参照してください。

## はじめに

このガイドで使用されるエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、[はじめにガイド](./getting-started.md)を見て、必要なヘッダーやAPI呼び出し例の読み方など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## 推定の生成方法

プロファイルストアへのレコードの取り込みが、総プロファイル数を5%以上増減する場合、サンプリングジョブをトリガしてカウントを更新する。 データサンプリングをトリガする方法は、取り込み方法によって異なります。

* **バッチ取り込み：バッチ取り込み** の場合、バッチをプロファイルストアに取り込んでから15分以内に、5%の増減のしきい値に達すると、ジョブが実行されてカウントが更新されます。
* **ストリーミング取り込み：データワークフロー** のストリーミングの場合、5%増加または減少のしきい値が満たされたかどうかを調べるために、1時間ごとにチェックが行われます。ジョブが存在する場合は、そのジョブが自動的にトリガされ、カウントが更新されます。

スキャンのサンプルサイズは、プロファイルストアのエンティティの全体数によって異なります。 これらのサンプルサイズを次の表に示します。

| プロファイルストア内のエンティティ数 | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

>[!NOTE]
>
>推定値は通常、10 ～ 15秒かかります。まず、記録の数が増えるので、概算と精製から始まります。

## プレビューの新規作成 {#create-preview}

新しいプレビューを作成するには、`/preview` エンドポイントに POST リクエストを送信します。

>[!NOTE]
>
>プレビュージョブが作成されると、予測ジョブが自動的に作成されます。 これら2つのジョブは同じIDを共有します。

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
        "predicateModel": "_xdm.context.profile"
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `predicateExpression` | データのクエリに使用する PQL 式です。 |
| `predicateType` | `predicateExpression`の下のクエリ式の述語の種類です。 現在、このプロパティに指定できる値は`pql/text`のみです。 |
| `predicateModel` | プロファイルデータが基づく[!DNL Experience Data Model] (XDM)スキーマクラスの名前。 |

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
| `state` | プレビュージョブの現在の状態です。最初に作成されたときは、「NEW」状態になります。 次に、処理が完了するまで「実行中」の状態になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `previewId` | 次の節で説明するように、予測またはプレビューを表示する際に参照用に使用されるプレビュージョブのID。 |

## 特定のプレビュー{#get-preview}の結果を取得

`/preview`エンドポイントにGETリクエストを送信し、リクエストパスにプレビューIDを指定することで、特定のプレビューに関する詳細な情報を取得できます。

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
| `results` | エンティティIDと関連IDのリスト。 提供されたリンクは、[プロファイルアクセスAPIエンドポイント](../../profile/api/entities.md)を使用して、指定されたエンティティを検索するのに使用できます。 |

## 特定の見積ジョブの結果の取得 {#get-estimate}

プレビュージョブを作成したら、そのジョブの`previewId`を`/estimate`エンドポイントへのGETリクエストのパスで使用して、セグメント定義に関する表示の統計情報(予測オーディエンスサイズ、信頼区間、エラー標準偏差など)を取得できます。

**API 形式**

```http
GET /estimate/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 予測ジョブは、プレビュージョブが作成され、2つのジョブが参照用に同じID値を共有している場合にのみトリガーされます。 特に、プレビュージョブの作成時に返される`previewId`値です。 |

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
| `estimatedNamespaceDistribution` | セグメント内のプロファイル数を識別名前空間別に分類したオブジェクトの配列。 1つのプロファイルが複数の名前空間に関連付けられる可能性があるので、名前空間別のプロファイルの合計数(各名前空間に表示される値を合計)は、プロファイル数指標より多くなる場合があります。 例えば、ある顧客が複数のチャネルで自社のブランドとやり取りした場合、複数の名前空間がその個々の顧客に関連付けられます。 |
| `state` | プレビュージョブの現在の状態です。処理が完了するまで、状態は「実行中」になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `_links.preview` | `state`が「RESULT_READY」の場合、このフィールドは予測を表示するURLを提供します。 |

## 次の手順

このガイドを読んだ後は、セグメント化APIを使用したプレビューと予測の使い方について、より深く理解する必要があります。 特定の名前空間またはプロファイルデータストア内のプロファイルフラグメントと結合プロファイルの合計数など、リアルタイム顧客プロファイルデータに関連する指標にアクセスする方法については、[プロファイルプレビュー(`/previewsamplestatus`)のガイド](../../profile/api/preview-sample-status.md)を参照してください。
