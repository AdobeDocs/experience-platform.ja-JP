---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントの作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 7f61cee8fb5160d0f393f8392b4ce2462d602981

---


# セグメントの作成

このドキュメントでは、セグメントAPIを使用してセグメント定義を開発、テスト、プレビューおよび保存するためのチュートリアルを [提供しま](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)す。

ユーザーインターフェイスを使用したセグメントの作成方法について詳しくは、『セグメントビルダー』ガイド [を参照してくださ](../ui/overview.md)い。

## はじめに

このチュートリアルでは、オーディエンスセグメントの作成に関わる様々なAdobe Experience Platformサービスについて理解しておく必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
- [Adobe Experience Platform Segmentation Service](../home.md):リアルタイム顧客オーディエンスデータから顧客セグメントを作成できます。
- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。

以下の節では、プラットフォームAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

## セグメント定義の作成

セグメント化の最初の手順は、セグメントを定義することです。セグメントは、セグメント定義と呼ばれる構成体 **で表されま**&#x200B;す。 セグメント定義は、プロファイルクエリ言語(PQL)で書き込まれたクエリをカプセル化するオブジェクトです。 このオブジェクトは **PQL述語とも呼ばれます**。 PQL述部は、リアルタイム顧客プロファイルに指定するレコードや時系列データに関連する条件に基づいて、セグメントのルールを定義します。 PQLクエリの記述の詳細については [](../pql/overview.md) 、『PQLガイド』を参照してください。

リアルタイム顧客プロファイルAPIでエンドポイントにPOSTリクエストを送信するこ `/segment/definitions` とで、新しいセグメント定義を作成できます。 次の例では、セグメントを正しく定義するために必要な情報など、定義リクエストの書式を設定する方法について説明します。

セグメント定義は、バッチセグメント化とストリーミングセグメント化の2つの方法で評価できます。 バッチセグメント化では、事前に設定されたスケジュールまたは評価が手動でトリガーされた場合にセグメントが評価され、ストリーミングセグメント化では、データがPlatformに取り込まれるとすぐにセグメントが評価されます。 このチュートリアルでは、バッチセグメント化 **を使用しま**&#x200B;す。 ストリーミングセグメント化について詳しくは、ストリーミングセグメン [ト化の概要を参照してくださ](../ui/streaming-segmentation.md)い。

**API形式**

```http
POST /segment/definitions
```

**リクエスト**

次のリクエストでは、「MyProfile」という名前のスキーマの新しいセグメント定義を作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/segment/definitions \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "My Sample Cart Abandons Segment Definition",
        "schema": {
            "name": "MyProfile",
        },
        "expression": {
            "type": "PQL",
            "format": "pql/text",
            "value": "xEvent.metrics.commerce.abandons.value > 0",
        },
        "mergePolicyId": "mpid1",
        "description": "This Segment represents those users who have abandoned a cart"
    }'
```

| プロパティ | 説明 |
| --------- | ------------ | 
| `name` | **必須.** セグメントを参照する一意の名前。 |
| `schema` | **必須.** セグメント内のスキーマに関連付けられているエンティティ。 またはフィールドで構 `id` 成さ `name` れます。 |
| `expression` | **必須.** セグメント定義に関するフィールド情報を含むエンティティ。 |
| `expression.type` | 式タイプ 現在、「PQL」のみがサポートされています。 |
| `expression.format` | 値の式構造を示します。 現在、次の形式がサポートされています。 <ul><li>`pql/text`:公開されたPQL文法に従った、セグメント定義のテキスト表現。  例：`workAddress.stateProvince = homeAddress.stateProvince`。</li></ul> |
| `expression.value` | に示す式の種類に適合するデータで `expression.format`す。 |
| `mergePolicyId` | 書き出したデータに使用するマージポリシーの識別子。 詳しくは、マージポリシーの構成 [ドキュメント](../../profile/api/merge-policies.md)。 |
| `description` | 定義の解読可能な説明。 |

**応答**

成功した場合は、新しく作成されたセグメント定義の詳細が返されます。この詳細には、後でこのチュートリアルで使用する、システム生成の読み取り `id` 専用セグメント定義も含まれます。

```json
{
    "id": "1234",
    "name": "My Sample Cart Abandons Segment Definition",
    "description": "This Segment represents those users who have abandoned a cart",
    "type": "PQL",
    "format": "pql/text",
    "expression": "xEvent.metrics.commerce.abandons.value > 0",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/core/ups/segment/definitions/1234"
        }
    }
}
```

## 見積もりとプレビューとオーディエンス

セグメントの定義を作成する際、リアルタイム顧客プロファイルの見積もりツールとプレビューツールを表示の概要レベルの情報に使用して、期待されるオーディエンスを確実に特定できます。 予測は、予測されるオーディエンスサイズや信頼区間など、セグメント定義の統計情報を提供します。 プレビューは、セグメント定義の資格を持つプロファイルのページ分割リストを提供し、結果を期待どおりの結果と比較できます。

オーディエンスを見積もり、プレビューすることで、PQL述語をテストし、最適化して、希望の結果を得て、更新したセグメント定義で使用できるようにすることができます。

セグメントをプレビューまたは見積もるために必要な手順は2つあります。

1. [ジョブのプレビュー](#create-a-preview-job)
2. [表示ジョブの](#view-an-estimate-or-preview) IDを使用したプレビュー予測

### 予測の生成方法

データサンプルを使用して、セグメントを評価し、見極めのためのプロファイル数を予測します。 新しいデータが毎朝（UTCの午前12時～午前2時）にメモリに読み込まれ、すべてのセグメントクエリがその日のサンプルデータを使用して推定されます。 その結果、新しく追加されたフィールドや収集されたデータは、翌日の予測に反映されます。

サンプルサイズは、エンティティストア内の全体のエンティティ数にプロファイルされます。 次の表に、これらのサンプルサイズを示します。

| エンティティストア内のプロファイル | サンプルサイズ |
| ------------------------- | ----------- |
| 100万未満 | フルデータセット |
| 1 ～ 2000万 | 100万 |
| 2千万以上 | 合計5% |

予測は通常、10 ～ 15秒を超え、大まかな予測と、より多くの記録が読み取られたための調整から始まります。

### ジョブのプレビュー

エンドポイントにPOSTリクエストをプレビューすることで、新しいエンドポイントジョブを作成で `/preview` きます。

**API形式**

```http
POST /preview
```

**リクエスト**

次のリクエストは、新しいジョブをプレビューします。 リクエスト本文には、セグメントに関連するクエリ情報が含まれています。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/preview \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
        "predicateExpression": "xEvent.metrics.commerce.abandons.value > 0",
        "predicateType": "pql/text",
        "predicateModel": "_xdm.context.profile",
        "graphType": "simple",
        "mergeStrategy": "simple"
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `predicateExpression` | データをクエリするPQL式。 |
| `predicateModel` | プロファイルデータの基になるXDMスキーマの名前。 |

**応答**

正常な応答は、新しく作成されたプレビュージョブの詳細（IDと現在の処理状態を含む）を返します。

```json
{
   "state": "RUNNING",
   "previewQueryId": "4a45e853-ac91-4bb7-a426-150937b6af5c",
   "previewQueryStatus": "RUNNING",
   "previewId": "MDoyOjRhNDVlODUzLWFjOTEtNGJiNy1hNDI2LTE1MDkzN2I2YWY1Yzo0Mg",
   "previewExecutionId": 42
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `state` | ジョブの現在のプレビュー状態。 処理が完了するまで「実行中」の状態になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `previewId` | 次の節で説明するように、プレビュージョブのID。推定またはプレビューを表示する際に参照目的で使用されます。 |

### 表示見積もりまたはプレビュー

推定プロセスとプレビュープロセスは、異なるクエリが完了するまでに異なる時間を要する可能性があるので、非同期で実行されます。 クエリの開始後は、API呼び出しを使用して、予測またはプレビューの進行に伴う現在の状態を取得(GET)できます。

リアルタイム顧客プロファイルAPIを使用して、IDでプレビュージョブの現在の状態を参照できます。 状態が「RESULT_READY」の場合は、結果を表示できます。 見積もりとプレビューのどちらを表示するかに応じて、APIにはそれぞれ独自のエンドポイントがあります。 両方の例を以下に示します。

### 表示見積もり

**API形式**

```http
GET /estimate/{PREVIEW_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{PREVIEW_ID}` | 表示するプレビュージョブのID。 |

**リクエスト**

次のリクエストは、前の手順で作成したを使用し `previewId` て予測を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/estimate/MDoyOjRhNDVlODUzLWFjOTEtNGJiNy1hNDI2LTE1MDkzN2I2YWY1Yzo0Mg \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、予測の詳細を返します。

```json
{
    "estimatedSize": 45,
    "state": "RESULT_READY",
    "profilesReadSoFar": 83834,
    "standardError": 0,
    "error": {
        "description": "",
        "traceback": ""
    },
    "profilesMatchedSoFar": 46,
    "totalRows": 82473,
    "confidenceInterval": "95%",
    "_links": {
        "preview": "https://platform.adobe.io/data/core/ups/preview?previewQueryId=f88bc056-ee48-40d5-9ddb-8865d7d6a0e0"
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `state` | ジョブの現在のプレビュー状態。 処理が完了するまで「実行中」となり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `_links.preview` | プレビュージョブの現在の状態が「RESULT_READY」の場合、この属性は予測を表示するURLを提供します。 |

### 表示プレビュー

**API形式**

```http
GET /preview/{PREVIEW_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{PREVIEW_ID}` | 表示するプレビュージョブのID。 |

**リクエスト**

次のリクエストは、前の手順で作成したプレビューを使 `previewId` 用して、ユーザーを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/preview/MDoyOjRhNDVlODUzLWFjOTEtNGJiNy1hNDI2LTE1MDkzN2I2YWY1Yzo0Mg \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、応答の詳細を返します。プレビュー。

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
      }
   ],
   "page": {
      "offset": 0,
      "size": 3
   }
}
```

## 次の手順

セグメント定義の開発、テスト、保存が完了したら、セグメントジョブを作成して、リアルタイム顧客プロファイルAPIを使用してオーディエンスを構築できます。 セグメント結果の評価とア [クセスに関するチュートリアルを参照し](./evaluate-a-segment.md) 、これを達成する方法の詳細な手順を確認してください。