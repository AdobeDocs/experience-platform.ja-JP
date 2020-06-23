---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントの作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 822f43b139b68b96b02f9a5fe0549736b2524ab7
workflow-type: tm+mt
source-wordcount: '1328'
ht-degree: 2%

---


# セグメントの作成

このドキュメントでは、 [セグメント化APIを使用してセグメント定義を開発、テスト、プレビュー、保存するためのチュートリアルを提供します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)。

ユーザーインターフェイスを使用したセグメントの作成方法について詳しくは、『 [セグメントビルダーガイド](../ui/overview.md)』を参照してください。

## はじめに

このチュートリアルでは、オーディエンスセグメントの作成に関連する様々なAdobe Experience Platformサービスについて、十分な理解が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [Adobe Experience Platformセグメントサービス](../home.md): リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
- [Experience Data Model(XDM)](../../xdm/home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。

以下の節では、PlatformAPIを正しく呼び出すために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] Platform内のサンドボックスについて詳しくは、「 [Sandboxの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## セグメント定義の作成

セグメント化の最初の手順は、セグメントを定義することです。セグメントを定義する手順は、 **セグメント定義と呼ばれる構成体で表されます**。 セグメント定義は、プロファイルクエリ言語(PQL)で記述されたクエリをカプセル化するオブジェクトです。 このオブジェクトは、 **PQL述語とも呼ばれます**。 PQL述語は、リアルタイム顧客プロファイルに指定するレコードまたは時系列データに関連する条件に基づいて、セグメントのルールを定義します。 PQLクエリの記述の詳細については、 [PQLガイド](../pql/overview.md) を参照してください。

リアルタイム顧客プロファイルAPIでエンドポイントにPOSTリクエストを作成して、新しいセグメント定義を作成でき `/segment/definitions` ます。 次の例は、セグメントを正しく定義するために必要な情報など、定義リクエストの書式設定方法を示しています。

セグメント定義は、バッチセグメント化とストリーミングセグメント化の2つの方法で評価できます。 バッチセグメントは、事前設定されたスケジュールに基づいて、または評価が手動でトリガされた場合にセグメントを評価し、ストリーミングセグメントは、Platform内のデータが取り込まれるとすぐにセグメントを評価します。 このチュートリアルでは、 **バッチセグメント化を使用します**。 ストリーミングセグメント化について詳しくは、ストリーミングセグメント化の [概要を参照してください](../api/streaming-segmentation.md)。

**API形式**

```http
POST /segment/definitions
```

**リクエスト**

次のリクエストは、「MyProfile」という名前のスキーマに対して新しいセグメント定義を作成します。

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
| `name` | **必須。** セグメントを参照する一意の名前。 |
| `schema` | **必須。** セグメント内のエンティティに関連付けられているスキーマ。 またはフィールドで構成され `id``name` ます。 |
| `expression` | **必須。** セグメント定義に関するフィールド情報を含むエンティティ。 |
| `expression.type` | 式タイプを指定します。 現在、「PQL」のみがサポートされています。 |
| `expression.format` | 式の構造を値で示します。 現在、次の形式がサポートされています。 <ul><li>`pql/text`: パブリッシュされたPQL文法に従った、セグメント定義のテキスト表現。  例：`workAddress.stateProvince = homeAddress.stateProvince`。</li></ul> |
| `expression.value` | に示す種類に適合する式 `expression.format`。 |
| `mergePolicyId` | エクスポートするデータに使用するマージポリシーの識別子。 詳しくは、 [マージポリシー設定ドキュメントを参照してください](../../profile/api/merge-policies.md)。 |
| `description` | 定義の人間が読み取り可能な説明。 |

**応答**

「正常に完了」の応答は、新たに作成されたセグメント定義の詳細を返します。このセグメント定義には、後でこのチュートリアルで使用する、システム生成の読み取り専用 `id` のものが含まれます。

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

## オーディエンスの見積もりとプレビュー {#estimate-and-preview-an-audience}

セグメント定義を作成する際、リアルタイム顧客プロファイル内の見積もりツールとプレビューツールを使用して表示の概要レベルの情報を提供し、期待されるオーディエンスを確実に特定できます。 予測は、予測されるオーディエンスサイズや信頼区間など、セグメント定義の統計情報を提供します。 プレビューは、セグメント定義の条件を満たすプロファイルのページ分割リストを提供し、期待どおりの結果と比較できます。

オーディエンスを見積もってプレビューすることで、PQL予測をテストして最適化し、必要な結果を得られるようにします。結果は、PQL予測をテストして最適化し、更新したセグメント定義で使用できます。

セグメントのプレビューや予測の取得に必要な手順は2つあります。

1. [プレビュージョブの作成](#create-a-preview-job)
2. [表示ジョブのIDを使用したプレビューの予測またはプレビュー](#view-an-estimate-or-preview)

### 予測の生成方法

データサンプルを使用して、セグメントを評価し、適切なプロファイルの数を見積もります。 新しいデータが毎朝メモリに読み込まれ(午前12時～午前2時（UTCの午前7時～午前9時）、すべてのセグメントクエリがその日のサンプルデータを使用して見積もられます。 したがって、新しく追加されたフィールドや収集されたデータが、翌日の予測に反映されます。

サンプルサイズは、プロファイルストア内のエンティティの全体数に依存します。 次の表に、これらのサンプルサイズを示します。

| プロファイルストア内のエンティティ | サンプルサイズ |
| ------------------------- | ----------- |
| 100万未満 | フルデータセット |
| 1～2千万 | 100万 |
| 2千万以上 | 合計5% |

推定時間は、おおまかな予想から10～15秒と、記録が読み上げられる中で、精製が始まる。

### プレビュージョブの作成

エンドポイントにPOSTリクエストを作成して、新しいプレビュージョブを作成でき `/preview` ます。

**API形式**

```http
POST /preview
```

**リクエスト**

次の要求は、新しいプレビュージョブを作成します。 リクエスト本文には、セグメントに関連するクエリ情報が含まれます。

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
| `state` | プレビュージョブの現在の状態。 処理が完了するまで「実行中」の状態になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `previewId` | 次の節で説明するように、予測またはプレビューを表示する際に参照用に使用されるプレビュージョブのID。 |

### 見積やプレビューの表示

見積もりプロセスとプレビュープロセスは、異なるクエリが完了するまでに異なる時間を要する場合があるので、非同期に実行されます。 クエリが開始されると、API呼び出しを使用して、予測またはプレビューの進行に伴う現在の状態を取得(GET)できます。

リアルタイム顧客プロファイルAPIを使用して、プレビュージョブのIDを使用してジョブの現在の状態を参照できます。 状態が「RESULT_READY」の場合、結果を表示できます。 予測とプレビューのどちらを表示するかに応じて、APIにはそれぞれ独自のエンドポイントがあります。 両方の例を次に示します。

### 表示と見積もり

**API形式**

```http
GET /estimate/{PREVIEW_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{PREVIEW_ID}` | 表示するプレビュージョブのID。 |

**リクエスト**

次のリクエストは、前の手順で `previewId` 作成した値を使用して予測を取得します。

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
| `state` | プレビュージョブの現在の状態。 処理が完了するまで「実行中」になり、その時点で「RESULT_READY」または「FAILED」になります。 |
| `_links.preview` | プレビュージョブの現在の状態が「RESULT_READY」の場合、この属性は予測を表示するためのURLを提供します。 |

### プレビューの表示

**API形式**

```http
GET /preview/{PREVIEW_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{PREVIEW_ID}` | 表示するプレビュージョブのID。 |

**リクエスト**

次のリクエストは、前の手順で `previewId` 作成したを使用してプレビューを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/preview/MDoyOjRhNDVlODUzLWFjOTEtNGJiNy1hNDI2LTE1MDkzN2I2YWY1Yzo0Mg \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、プレビューの詳細が返されます。

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

セグメント定義の開発、テスト、保存が完了したら、セグメントジョブを作成して、リアルタイム顧客プロファイルAPIを使用してオーディエンスを作成できます。 これを行う方法の詳細手順については、セグメント結果の [評価とアクセスに関するチュートリアル](./evaluate-a-segment.md) を参照してください。