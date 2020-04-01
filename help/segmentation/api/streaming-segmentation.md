---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミングセグメント
topic: developer guide
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc

---


# ストリーミングイベント（ベータ版）を使用して、セグメントをリアルタイムで評価します。

>[!NOTE] ストリーミングセグメントはベータ版の機能で、ご要望に応じてご利用いただけます。

ストリーミングセグメント化(連続クエリ評価とも呼ばれます)は、イベントが特定のセグメントグループに入った直後に顧客を即座に評価する機能です。 この機能を使用すると、ほとんどのセグメントルールをAdobe Experience Platformに渡す際に評価できるようになりました。つまり、セグメントのメンバーシップは、スケジュール済みのセグメントジョブを実行しない限り最新の状態に保たれます。

![](../images/api/streaming-segment-evaluation.png)

## はじめに

この開発者ガイドでは、ストリーミングセグメント化に関連する様々なAdobe Experience Platformサービスについて、実際に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計プロファイルに基づいて、統合された顧客データをリアルタイムで提供します。
- [セグメント](../home.md):リアルタイム顧客データからセグメントやオーディエンスを作成する機能を提供します。
- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。

以下の節では、プラットフォームAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

この開発者ガイドは、リクエストの形式を設定する方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

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

特定のリクエストを完了するには、追加のヘッダーが必要な場合があります。 このドキュメントの各例では、正しいヘッダーが表示されます。 必要なヘッダーがすべて含まれるように、サンプルリクエストに特に注意してください。

### ストリーミングセグメント化が有効なクエリタイプ

次の表に、様々なタイプのセグメントクエリと、ストリーミングセグメントをサポートしているかどうかを示します。

| クエリ型 | サンプルクエリ | ストリーミングセグメントのサポート |
| ---------- | ------------ | --------------------------------- |
| 簡単な人口統計 | 「カナダに住所を持つ人々を全員教えて下さい」 | サポート |
| 時系列イベント | 「Lightroomをダウンロードしたすべての人を知らせてください。」 | サポート |
| 人口統計と時系列 | 「カナダに住んで、過去30日間に注文した人を全員私にくれ」 | サポート |
| イベント | 「お互いに2日以内に別々の買い物かごを2台放棄した人を全員私にくれ」 | サポート |
| マルチエンティティ | 「エンタイトルメントのタイプが「エクスペリエンス」の人を全員教えてください」 | サポートなし |
| 高度なPQL関数 | 「先週注文したプロファイルを全部教えて、購入したすべての製品のSKUと名前を含めます。」 | サポートなし |

## すべてのストリーミングセグメントが有効なセグメントの取得

新しいストリーミング対応セグメントを作成する前、または既存のセグメントをストリーミング対応に更新する前に、すべてのストリーミング対応セグメントのリストを取得して、情報が重複していないことを確認してください。

**API形式**

ストリーミングが有効なセグメントを取得するには、リクエストパスにクエリパ `evaluationInfo.continuous.enabled=true` ラメーターを含める必要があります。

```http
GET /segment/definitions?evaluationInfo.continuous.enabled=true
```

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/segment/definitions?evaluationInfo.continuous.enabled=true' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME'
```

**応答**

成功した応答は、ストリーミングセグメント化が有効なセグメントの配列をIMS組織に返します。

```json
{
    "segments": [
        {
            "id": "15063cb-2da8-4851-a2e2-bf59ddd2f004",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
            "imsOrgId": "{IMS_ORG_ID}",
            "sandbox": {
                "sandboxId": "",
                "sandboxName": "",
                "type": "production",
                "default": true
            },
            "name": " People who are NOT on their homepage ",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = false"
            },
            "evaluationInfo": {
                "batch": {
                    "enabled": false
                },
                "continuous": {
                    "enabled": true
                },
                "synchronous": {
                    "enabled": false
                }
            },
            "creationTime": 1572029711000,
            "updateEpoch": 1572029712000,
            "updateTime": 1572029712000
        },
        {
            "id": "f15063cb-2da8-4851-a2e2-bf59ddd2f004",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
            "imsOrgId": "{IMS_ORG_ID}",
            "sandbox": {
                "sandboxId": "",
                "sandboxName": "",
                "type": "production",
                "default": true
            },
            "name": "Homepage_continuous",
            "description": "People who are on their homepage - continuous",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
            },
            "evaluationInfo": {
                "batch": {
                    "enabled": true
                },
                "continuous": {
                    "enabled": true
                },
                "synchronous": {
                    "enabled": false
                }
            },
            "creationTime": 1572021085000,
            "updateEpoch": 1572021086000,
            "updateTime": 1572021086000
        }
    ],
    "page": {
        "totalCount": 2,
        "totalPages": 1,
        "sortField": "creationTime",
        "sort": "desc",
        "pageSize": 2,
        "limit": 100
    },
    "link": {}
}
```

## ストリーミングが有効なセグメントの作成

作成するセグメントがまだ存在しないことを確認したら、ストリーミングセグメント化を有効にする新しいセグメントを作成できます。

**API形式**

```http
POST /segment/definitions
```

**リクエスト**

次のリクエストでは、新しいセグメントが作成され、セグメントのストリーミングが有効になります。 Note that the `continuous` section is set to `enabled: true`.

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/segment/definitions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'  \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 30,
    "name": "Homepage_continuous",
    "description": "People who are on their homepage - continuous",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
    },
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": true
        },
        "synchronous": {
            "enabled": false
        }
    }
}'
```

>[!NOTE] これは、セクションのパラメーターを追加してに設定する、標準の「セグメントを作成」 `continuous` リクエストで `enabled: true`す。 セグメント定義の作成の詳細については、セグメントの作成に関するドキュメントを参照 [してくださ](../tutorials/create-a-segment.md)い。

**応答**

正常に応答すると、新たに作成されたストリーミングが有効なセグメント定義の詳細が返されます。

```json
{
    "id": "f15063cb-2da8-4851-a2e2-bf59ddd2f004",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 30,
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "{SANDBOX_ID}",
        "sandboxName": "{SANDBOX_NAME}",
        "type": "production",
        "default": true
    },
    "name": "Homepage_continuous",
    "description": "People who are on their homepage - continuous",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
    },
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": true,
                   },
        "synchronous": {
            "enabled": false
        }
    },
    "creationTime": 1572021085000,
    "updateEpoch": 1572021086000,
    "updateTime": 1572021086000
}
```

## 既存のセグメントをストリーミングセグメントに対して有効にする

セグメント定義のIDをPATCHリクエストのパスに指定することで、既存のセグメントをストリーミングセグメント化に有効にできます。 さらに、このPATCHリクエストのペイロードには、既存のセグメント定義の完全な詳細を含める必要があります。この詳細は、該当するセグメント定義にGETリクエストを行うことでアクセスできます。

### 既存のセグメント定義の検索

既存のセグメント定義を検索するには、GETリクエストのパスにそのIDを指定する必要があります。

**API形式**

```http
GET /segment/definitions/{SEGMENT_DEFINITION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_DEFINITION_ID}` | 検索するセグメント定義のID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/segment/definitions/15063cb-2da8-4851-a2e2-bf59ddd2f004\
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、リクエストしたセグメント定義の詳細が返されます。

```json
{
    "id": "15063cb-2da8-4851-a2e2-bf59ddd2f004",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "sandbox": {
        "sandboxId": "",
        "sandboxName": "",
        "type": "production",
        "default": true
    },
    "name": "TestStreaming1",
    "expression": {
        "type": "PQL",
        "format": "pql/json",
        "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
    },
    "mergePolicyId": "50de2f9c-990c-4b96-945f-9570337ffe6d",
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": false
        },
        "synchronous": {
            "enabled": false
        }
    }
}
```

>[!NOTE] 次のリクエストでは、この応答で返されたセグメント定義の詳細が必要です。 この応答の詳細をコピーして、次の要求の本文で使用してください。

### 既存のセグメントをストリーミングセグメント化に有効化

更新するセグメントの詳細がわかったので、PATCHリクエストを実行してセグメントを更新し、ストリーミングセグメントを有効にすることができます。

**API形式**

```http
PATCH /segment/definitions/{SEGMENT_DEFINITION_ID}
```

**リクエスト**

次のリクエストのペイロードは、(前の手順で取得した [)セグメント定義の詳細を提供し、そのプロパティをに変更して更新](#look-up-an-existing-segment-definition)します `continuous.enabled``true`。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/core/ups/segment/definitions/15063cb-2da8-4851-a2e2-bf59ddd2f004 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG_ID}' \
  -d '{
    "id": "15063cb-2da8-4851-a2e2-bf59ddd2f004",
    "schema": {
        "name": "_xdm.context.profile"
    },
    
    "sandbox": {
        "sandboxId": "{SANDBOX_ID}",
        "sandboxName": "{SANDBOX_NAME}",
        "type": "production",
        "default": true
    },
    "name": "TestStreaming1",
    "expression": {
        "type": "PQL",
        "format": "pql/json",
        "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
    },
    "mergePolicyId": "50de2f9c-990c-4b96-945f-9570337ffe6d",
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": true
        },
        "synchronous": {
            "enabled": false
        }
    }
}'
```

**応答**

正常に完了すると、新しく更新されたセグメント定義の詳細が返されます。

```json
{
    "id": "15063cb-2da8-4851-a2e2-bf59ddd2f004",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 30,
    "imsOrgId": "4A21D36B544916100A4C98A7@AdobeOrg",
    "sandbox": {
        "sandboxId": "{SANDBOX_ID}",
        "sandboxName": "{SANDBOX_NAME}",
        "type": "production",
        "default": true
    },
    "name": "TestStreaming1",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
    },
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": true
        },
        "synchronous": {
            "enabled": false
        }
    },
    "creationTime": 1572029711000,
    "updateEpoch": 1572029712000,
    "updateTime": 1572029712000
}
```

## スケジュールされた評価の有効化

ストリーミング評価が有効になったら、ベースラインを作成する必要があります（ベースラインの作成後、セグメントは常に最新の状態になります）。 これはシステムによって自動的に行われますが、ベースライニングを行うには、最初にスケジュールされた評価（スケジュールされたセグメント化）を有効にする必要があります。

スケジュール済みセグメントを使用すると、IMS組織で繰り返しスケジュールを作成して、セグメントを評価する書き出しジョブを自動的に実行できます。

>[!NOTE] XDM個々のサンドボックスに対して、最大5個のマージポリシーを持つサンドボックスに対して、スケジュールされた評価を有効にすることができます。プロファイル 1つのサンドボックス環境内にXDM個々のプロファイルに対して5つ以上の結合ポリシーがある場合、スケジュールされた評価を使用できません。

### スケジュールの作成

エンドポイントにPOSTリクエストを行うこ `/config/schedules` とで、スケジュールを作成し、スケジュールをトリガーする特定の時間を含めることができます。

**API形式**

```http
POST /config/schedules
```

**リクエスト**

次のリクエストは、ペイロードで提供された仕様に基づいて新しいスケジュールを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/schedules \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "{SCHEDULE_NAME}",
        "type": "batch_segmentation",
        "properties": {
            "segments": ["*"]
        },
        "schedule": "0 0 1 * * ?",
        "state": "inactive"
        }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | **（必須）** 、スケジュールの名前。 文字列である必要があります。 |
| `type` | **（必須）** 、文字列形式のジョブタイプ。 サポートされているタイプはと `batch_segmentation` です `export`。 |
| `properties` | **（必須）** 、集計表に関連する追加のプロパティを含むオブジェクト。 |
| `properties.segments` | **(次に等しい場合に必須`type`)を使用す`batch_segmentation`ると** 、すべてのセグメントが `["*"]` 確実に含まれます。 |
| `schedule` | **（必須）** 、ジョブスケジュールを含む文字列。 ジョブは1日に1回のみ実行するようにスケジュールできます。つまり、24時間に1回以上実行するようにジョブをスケジュールすることはできません。 例(`0 0 1 * * ?`)は、ジョブが毎日1:00:00 UTCにトリガーされることを意味します。 詳しくは、 [cron式形式のドキュメント](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 |
| `state` | *（オプション）* 、スケジュールの状態を含む文字列。 使用可能な値： `active` と `inactive`デフォルト値は `inactive` です。IMS組織で作成できるスケジュールは1つだけです。 スケジュールを更新する手順は、このチュートリアルの後半で説明します。 |

**応答**

正常に応答すると、新しく作成されたスケジュールの詳細が返されます。

```json
{
    "id": "cd585edf-962d-420d-94ad-3be03e619ac2",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "e7e17720-c5bb-11e9-aafb-87c71c35cac8",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "{SCHEDULE_NAME}",
    "state": "inactive",
    "type": "batch_segmentation",
    "schedule": "0 0 1 * * ?",
    "properties": {
        "segments": [
            "*"
        ]
    },
    "createEpoch": 1568267948,
    "updateEpoch": 1568267948
}
```

### スケジュールの有効化

デフォルトでは、作成(POST)要求本文でプロパティがに設定さ `state` れていない限り、スケジュ `active` ールは作成時に非アクティブになります。 エンドポイントにPATCHリクエストを行い、パスにスケジ `state` ュールのIDを含めることで、スケジュールを有効にする( `active`に設定する)こ `/config/schedules` とができます。

**API形式**

```http
POST /config/schedules/{SCHEDULE_ID}
```

**リクエスト**

次のリクエストでは、 [JSONパッチの形式設定を使用し](http://jsonpatch.com/) 、スケジュールの更新先 `state` としてスケジュールを更新しま `active`す。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/schedules/cd585edf-962d-420d-94ad-3be03e619ac2 \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        {
          "op": "add",
          "path": "/state",
          "value": "active"
        }
      ]'
```

**応答**

更新が成功すると、空の応答本文とHTTPステータス204（コンテンツなし）が返されます。

同じ操作を使用して、前のリクエストの「値」を「inactive」に置き換えることで、スケジュールを無効にできます。

## 次の手順

これで、新しいセグメントと既存のセグメントの両方を有効にし、スケジュールされたセグメントを有効にしてベースラインを作成し、定期的な評価を実行できるようになりました。組織のセグメントの作成を開始できます。

Adobe Experience Platformユーザーインターフェイスを使用して同様のアクションを実行し、セグメントを操作する方法については、『セグメントビルダー』ユ [ーザーガイドを参照してくださ](../ui/overview.md)い。