---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: エンドポイントのプレビューと予測
topic: developer guide
translation-type: tm+mt
source-git-commit: b3e6a6f1671a456b2ffa61139247c5799c495d92
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 2%

---


# エンドポイントのプレビューと予測

セグメント定義を作成する際に、の見積もりツールとプレビューツールを使用して、表示の概要レベルの情報 [!DNL Adobe Experience Platform] を得ることで、期待されるオーディエンスを確実に分離できます。 **プレビューには** 、セグメント定義の条件を満たすプロファイルのページ分割リストが用意されており、期待どおりの結果と比較できます。 **予測は** 、予測されるオーディエンスサイズ、信頼区間、誤差の標準偏差など、セグメント定義の統計情報を提供します。

## はじめに

このガイドで使用されるエンドポイントは、 [!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、 [入門ガイドを参照して](./getting-started.md) 、必要なヘッダーやAPI呼び出し例を読む方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## 予測の生成方法

データサンプリングがトリガされる方法は、取り込み方法によって異なります。

バッチ取り込みの場合、プロファイルストアは15分ごとに自動的にスキャンされ、最後のサンプリングジョブが実行されてから新しいバッチが正常に取り込まれたかどうかを確認します。 その場合、プロファイルストアがスキャンされ、レコード数に少なくとも5%の変更があったかどうかが確認されます。 これらの条件が満たされると、新しいサンプリングジョブがトリガされます。

ストリーミング取り込みの場合、プロファイルストアは1時間ごとに自動的にスキャンされ、レコード数に少なくとも5%の変更があったかどうかが確認されます。 この条件が満たされると、新しいサンプリングジョブがトリガされます。

スキャンのサンプルサイズは、プロファイルストアのエンティティの全体数によって異なります。 次の表に、これらのサンプルサイズを示します。

| プロファイルストア内のエンティティ | サンプルサイズ |
| ------------------------- | ----------- |
| 100万未満 | フルデータセット |
| 1～2千万 | 100万 |
| 2千万以上 | 合計5% |

>[!NOTE] 推定値は通常、10 ～ 15秒かかります。まず、記録の数が増えるので、概算と精製から始まります。

## Create a new preview {#create-preview}

エンドポイントにPOSTリクエストを作成して、新しいプレビューを作成でき `/preview` ます。

>[!NOTE] プレビュージョブが作成されると、予測ジョブが自動的に作成されます。 これら2つのジョブは同じIDを共有します。

**API形式**

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
| `predicateExpression` | データをクエリするPQL式。 |
| `predicateType` | の下にあるクエリ式の述語の種類 `predicateExpression`です。 現在、このプロパティに指定できる値は `pql/text`です。 |
| `predicateModel` | プロファイルデータの基になるExperience Data Model(XDM)スキーマの名前。 |

**応答**

正常に応答すると、新しく作成したプレビューの詳細と共に、HTTPステータス201（作成済み）が返されます。

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
| `state` | プレビュージョブの現在の状態。 最初に作成されたときは、「NEW」状態になります。 次に、処理が完了するまで「実行中」の状態になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `previewId` | 次の節で説明するように、予測またはプレビューを表示する際に参照用に使用されるプレビュージョブのID。 |

## 特定のプレビューの結果を取得する {#get-preview}

エンドポイントにGETリクエストを送信し、リクエストパスにプレビューIDを指定することで、特定のプレビューに関する詳細な情報を取得でき `/preview` ます。

**API形式**

```http
GET /preview/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 取得するプレビューの `previewId` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/preview/MDphcHAtMzJiZTAzMjgtM2YzMS00YjY0LThkODQtYWNkMGM0ZmJkYWQzOmU4OTAwNjhiLWY1Y2EtNGE4Zi1hNmI1LWFmODdmZjBjYWFjMzow \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定されたプレビューに関する詳細情報と共にHTTPステータス200を返します。

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
        },
        "state": "RESULT_READY",
        "links": {
            "_self": "https://platform.adobe.io/data/core/ups/preview?expression=<expr-1>&limit=1000",
            "next": "",
            "prev": ""
        }
    }],
    "page": {
        "offset": 0,
        "size": 3
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `results` | エンティティIDと関連IDのリスト。 提供されたリンクは、 [プロファイルアクセスAPIを使用して、指定されたエンティティを検索するために使用できます](../../profile/api/entities.md)。 |

## 特定の見積ジョブの結果を取得します {#get-estimate}

プレビュージョブを作成したら、そのジョブをエンドポイント `previewId``/estimate` へのGETリクエストのパスで使用して、セグメント定義に関する表示の統計情報(予測オーディエンスサイズ、信頼区間、エラー標準偏差など)を取得できます。

**API形式**

```http
GET /estimate/{PREVIEW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{PREVIEW_ID}` | 予測ジョブは、プレビュージョブが作成され、2つのジョブが参照用に同じID値を共有している場合にのみトリガーされます。 特に、これはプレビュージョブの作成時に返される `previewId` 値です。 |

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

正常な応答は、予測ジョブの詳細と共にHTTPステータス200を返します。

```json
{
    "estimatedSize": 0,
    "numRowsToRead": 1,
    "state": "RESULT_READY",
    "profilesReadSoFar": 1,
    "standardError": 0,
    "error": {
        "description": "",
        "traceback": ""
    },
    "profilesMatchedSoFar": 0,
    "totalRows": 1,
    "confidenceInterval": "95%",
    "_links": {
        "preview": "https://platform.adobe.io/data/core/ups/preview/app-32be0328-3f31-4b64-8d84-acd0c4fbdad3/execution/0?previewQueryId=e890068b-f5ca-4a8f-a6b5-af87ff0caac3"
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `state` | プレビュージョブの現在の状態。 処理が完了するまで「実行中」になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `_links.preview` | プレビュージョブの現在の状態が「RESULT_READY」の場合、この属性は予測を表示するためのURLを提供します。 |

## 次の手順

このガイドを読むと、プレビューと予測の使い方に関する理解が深まります。 その他のSegmentation Service APIエンドポイントについて詳しくは、 [Segmentation Service開発者ガイドの概要を参照してください](./overview.md)。