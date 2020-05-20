---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミングセグメント
topic: developer guide
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc
workflow-type: tm+mt
source-wordcount: '1402'
ht-degree: 2%

---


# ストリーミングセグメント（ベータ版）を使用してイベントをリアルタイムで評価します。

>[!NOTE] ストリーミングセグメントはベータ版機能で、ご要望に応じてご利用いただけます。

ストリーミングセグメント(継続的なクエリ評価とも呼ばれます)は、イベントが特定のセグメントグループに入った直後に顧客を即座に評価する機能です。 この機能により、ほとんどのセグメントルールをAdobe Experience Platformに渡すデータと同時に評価できるようになりました。つまり、セグメント化ジョブをスケジュールせずに、セグメントのメンバーシップが最新の状態に維持されます。

![](../images/api/streaming-segment-evaluation.png)

## はじめに

この開発者ガイドでは、ストリーミングのセグメント化に関連する様々なAdobe Experience Platformサービスについて、十分に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統一された消費者プロファイルを提供します。
- [セグメント](../home.md): リアルタイムの顧客プロファイルデータからセグメントやオーディエンスを作成できます。
- [Experience Data Model(XDM)](../../xdm/home.md): プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。

以下の節では、プラットフォームAPIの呼び出しを正常に行うために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

この開発者ガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

特定のリクエストを完了するには、追加のヘッダーが必要になる場合があります。 このドキュメントの各例には、正しいヘッダーが表示されます。 必要なヘッダーがすべて含まれていることを確認するため、サンプルリクエストには特に注意してください。

### ストリーミングセグメント化が有効なクエリタイプ

次の表に、様々なタイプのセグメントクエリと、ストリーミングセグメントをサポートしているかどうかをリストします。

| クエリ型 | サンプルクエリ | ストリーミングセグメントのサポート |
| ---------- | ------------ | --------------------------------- |
| 単純人口統計 | 「カナダに住所を持つ人を全員教えて下さい」 | サポート |
| 時系列イベント | 「Lightroomをダウンロードしたすべての人を教えてください。」 | サポート |
| 人口統計と時系列 | 「カナダに住んで、過去30日間に注文した人を全員私にくれ」 | サポート |
| イベントの欠如 | 「2日以内に別々の買い物かごを2つ捨てた人を全員私にくれ」 | サポート |
| マルチエンティティ | 「エンタイトルメントのタイプが「エクスペリエンス」である人を全員教えてください」 | サポートなし |
| 高度なPQL関数 | 「先週注文したプロファイルを全部教えて、購入したすべての製品のSKUと名前を含めてください。」 | サポートなし |

## すべてのストリーミングセグメント化が有効なセグメントの取得

新しいストリーミング対応セグメントを作成する前、または既存のセグメントをストリーミング対応に更新する前に、すべてのストリーミング対応セグメントのリストを取得して、情報が重複していないことを確認してください。

**API形式**

ストリーミングが有効なセグメントを取得するには、リクエストパスにクエリパラメーター `evaluationInfo.continuous.enabled=true` を含める必要があります。

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

成功した応答は、ストリーミングセグメント化が有効なIMS組織内のセグメントの配列を返します。

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

作成するセグメントが存在しないことを確認したら、ストリーミングセグメントを有効にする新しいセグメントを作成できます。

**API形式**

```http
POST /segment/definitions
```

**リクエスト**

次のリクエストは、ストリーミングセグメント化が有効な新しいセグメントを作成します。 Note that the `continuous` section is set to `enabled: true`.

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

>[!NOTE] これは、標準の「セグメントを作成」リクエストで、 `continuous` セクションのパラメーターがに設定され `enabled: true`ます。 セグメント定義の作成の詳細については、セグメント作成に関するドキュメントを参照して [ください](../tutorials/create-a-segment.md)。

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

## ストリーミングセグメント用の既存のセグメントの有効化

セグメント定義のIDをPATCHリクエストのパスに指定することで、既存のセグメントを有効にしてセグメントをストリーミングできます。 さらに、このPATCHリクエストのペイロードには、既存のセグメント定義の完全な詳細を含める必要があります。これは、該当するセグメント定義にGETリクエストを行うことでアクセスできます。

### 既存のセグメント定義の検索

既存のセグメント定義を検索するには、GETリクエストのパスに既存のセグメント定義のIDを指定する必要があります。

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

>[!NOTE] 次のリクエストでは、この応答で返されたセグメント定義の完全な詳細が必要になります。 この応答の詳細をコピーして、次の要求の本文に使用してください。

### ストリーミングセグメント用の既存のセグメントの有効化

更新するセグメントの詳細がわかったら、PATCHリクエストを実行してセグメントを更新し、ストリーミングセグメントを有効にできます。

**API形式**

```http
PATCH /segment/definitions/{SEGMENT_DEFINITION_ID}
```

**リクエスト**

次のリクエストのペイロードは、( [前の手順で取得した](#look-up-an-existing-segment-definition))セグメント定義の詳細を指定し、その `continuous.enabled` プロパティをに変更して更新しま `true`す。

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

ストリーミング評価を有効にしたら、ベースラインを作成する必要があります（ベースラインを作成した後、セグメントは常に最新の状態になります）。 これはシステムによって自動的に行われますが、ベースライン設定を行うには、スケジュールされた評価（スケジュールされたセグメント化とも呼ばれます）を最初に有効にする必要があります。

スケジュール済みセグメントを使用すると、IMS組織で繰り返しスケジュールを作成して、セグメントを評価するために書き出しジョブを自動的に実行できます。

>[!NOTE] スケジュールされた評価は、XDM個別プロファイル用に最大5個のマージポリシーを持つサンドボックスに対して有効にできます。 1つのSandbox環境内にXDM個々のプロファイル用に5つ以上のマージポリシーがある場合、スケジュールされた評価を使用できません。

### スケジュールの作成

エンドポイントにPOSTリクエストを行うことで、スケジュールを作成し、スケジュールをトリガーする特定の時間を含めることがで `/config/schedules` きます。

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
| `type` | **（必須）** 。文字列形式のジョブタイプ。 サポートされているタイプは `batch_segmentation` およびで `export`す。 |
| `properties` | **（必須）** 、集計表に関連する追加のプロパティを含むオブジェクト。 |
| `properties.segments` | **(`type`等しい場合に必須)を`batch_segmentation`使用すると**`["*"]` 、すべてのセグメントが確実に含まれます。 |
| `schedule` | **（必須）** 、ジョブスケジュールを含む文字列。 ジョブは、1日に1回しか実行するようにスケジュールできません。つまり、24時間にジョブを2回以上実行するようにスケジュールすることはできません。 次の例(`0 0 1 * * ?`)は、ジョブが毎日1:00:00 UTCでトリガーされることを意味します。 詳しくは、 [cron式形式のドキュメントを参照してください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 |
| `state` | *（オプション）* 、スケジュールの状態を含む文字列。 使用可能な値： `active` と `inactive`。 デフォルト値は `inactive` です。IMS組織で作成できるスケジュールは1つだけです。 スケジュールを更新する手順は、このチュートリアルの後半で説明します。 |

**応答**

正常に応答すると、新たに作成されたスケジュールの詳細が返されます。

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

デフォルトでは、スケジュールは作成時(POST)のリクエスト本文で `state` プロパティがに設定されていない限り、作成時 `active` には非アクティブになります。 エンドポイントにPATCHリクエストを行い、 `state` パスにスケジュールのIDを含めることで、スケジュールを有効にする( `active`toに設定する `/config/schedules` )ことができます。

**API形式**

```http
POST /config/schedules/{SCHEDULE_ID}
```

**リクエスト**

次の要求では、 [JSONパッチの形式設定を使用して](http://jsonpatch.com/) 、スケジュール `state` の更新先を指定し `active`ます。

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

これで、新しいセグメントと既存のセグメントの両方をストリーミングセグメント用に有効にし、スケジュールされたセグメントを有効にしてベースラインを作成し、定期的な評価を実行できるようになりました。

Adobe Experience Platformユーザーインターフェイスを使用して、同様のアクションを実行し、セグメントを操作する方法については、 [セグメントビルダーユーザーガイドを参照してください](../ui/overview.md)。