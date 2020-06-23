---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミングセグメント
topic: developer guide
translation-type: tm+mt
source-git-commit: 822f43b139b68b96b02f9a5fe0549736b2524ab7
workflow-type: tm+mt
source-wordcount: '1343'
ht-degree: 1%

---


# ストリーミングセグメント化により、ほぼリアルタイムでイベントを評価

セグメント化のストリーミング [!DNL Adobe Experience Platform] により、お客様はデータの豊富性に重点を置きながら、ほぼリアルタイムでセグメント化を行うことができます。 ストリーミングセグメント化では、データが到着する際にセグメントの認定が行われ、セグメント化ジョブのスケジュール [!DNL Platform]や実行の必要性が軽減されました。 この機能を使用すると、ほとんどのセグメントルールをデータが渡されると評価できるようになりました。つまり、セグメントのメンバーシップは、スケジュール済みのセグメントジョブを実行しなくても最新の状態に維持されます。 [!DNL Platform]

![](../images/api/streaming-segment-evaluation.png)

## はじめに

この開発者ガイドでは、ストリーミングセグメント化に関連する様々な [!DNL Adobe Experience Platform] サービスについて、十分に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統一された消費者プロファイルを提供します。
- [!DNL Segmentation](../home.md): データからセグメントやオーディエンスを作成する機能を提供し [!DNL Real-time Customer Profile] ます。
- [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

以下の節では、APIの呼び出しを正常に行うために知っておく必要がある追加情報について説明し [!DNL Platform] ます。

### サンプルAPI呼び出しの読み取り

この開発者ガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

特定のリクエストを完了するには、追加のヘッダーが必要になる場合があります。 このドキュメントの各例には、正しいヘッダーが表示されます。 必要なヘッダーがすべて含まれていることを確認するため、サンプルリクエストには特に注意してください。

### ストリーミングセグメント化が有効なクエリタイプ {#streaming-segmentation-query-types}

>[!NOTE] ストリーミングセグメントを機能させるには、組織でスケジュールされたセグメント化を有効にする必要があります。 スケジュール済みセグメントを有効にする方法については、「スケジュール済みセグメントを [有効にする」の節を参照してください。](#enable-scheduled-segmentation)

ストリーミングセグメントを使用してセグメントを評価するには、クエリが次のガイドラインに従う必要があります。

| クエリ型 | 詳細 |
| ---------- | ------- |
| 受信ヒット | 時間制限のない、単一の着信イベントを参照するセグメント定義。 |
| 相対時間枠内での着信ヒット | 過去7日間に発生した単一のイベントを参照す **るセグメント定義**。 |
| プロファイルを参照する着信ヒット | 時間制限のない、1つの着信イベント、および1つ以上のプロファイル属性を参照するセグメント定義。 |
| 相対的な時間枠内のプロファイルを参照する着信ヒット | 過去7日間の、1つの着信イベントと1つ以上のプロファイル属性を参照す **るセグメント定義**。 |
| プロファイルを参照する複数のイベント | 過去24時間以内に複数のイベントを参照するセグメント定義 **には** 、1つ以上のプロファイル属性が含まれます。 |

次の節では、ストリーミングセグメントに対して **有効にしないリストセグメント定義の例を示します** 。

| クエリ型 | 詳細 |
| ---------- | ------- | 
| 相対時間枠内での着信ヒット | セグメント定義が、 **過去7日間** 以内でない着信イベントを参照する場合 ****。 例えば、 **過去2週間以内の場合**。 |
| 相対的なウィンドウ内のプロファイルを参照する着信ヒット | 次のオプションは、ストリーミングセグメントをサポートし **ません** 。<ul><li>過去7日間 **以** 内の着信イベント ****。</li><li>Adobe Audience Manager(AAM)のセグメントまたは特性を含むセグメント定義。</li></ul> |
| プロファイルを参照する複数のイベント | 次のオプションは、ストリーミングセグメントをサポートし **ません** 。<ul><li>過去24時間以内に **発生しないイベント******。</li><li>Adobe Audience Manager(AAM)のセグメントまたは特性を含むセグメント定義。</li></ul> |
| マルチエンティティクエリ | マルチエンティティクエリは、全体として、ストリーミングセグメントでは **サポートされていません** 。 |

さらに、ストリーミングセグメント化を行う際には、次のようなガイドラインが適用されます。

| クエリ型 | ガイドライン |
| ---------- | -------- |
| 単一イベントクエリ | ルックバックウィンドウは **7日間に制限されています**。 |
| イベント履歴のあるクエリ | <ul><li>ルックバックウィンドウは **1日に制限されます**。</li><li>イベント間に厳密な時間順序条件 **が存在する** 。</li><li>イベント間の単純な時間順（前後）のみが許可されます。</li><li>個々のイベント **を無効にすることはできません** 。 ただし、クエリ全体を無効にす **ることはできます** 。</li></ul> |

## ストリーミングセグメントで有効になっているすべてのセグメントを取得

エンドポイントにGETリクエストを行うことで、IMS組織内でストリーミングセグメント化が有効になっているすべてのセグメントのリストを取得でき `/segment/definitions` ます。

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
  -H 'x-sandbox-name: {SANDBOX_NAME}'
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

上記のいずれかのストリー [ミングセグメントタイプと一致する場合、セグメントは自動的にストリーミングが有効になります](#streaming-segmentation-query-types)。

**API形式**

```http
POST /segment/definitions
```

**リクエスト**

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
    }
}'
```

>[!NOTE] これは、標準の「セグメントの作成」リクエストです。 セグメント定義の作成の詳細については、セグメントの [作成に関するチュートリアルを参照してください](../tutorials/create-a-segment.md)。

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

## スケジュールされた評価の有効化 {#enable-scheduled-segmentation}

ストリーミング評価を有効にしたら、ベースラインを作成する必要があります（ベースラインを作成した後、セグメントは常に最新の状態になります）。 システムが自動的にベースライン設定を実行するには、スケジュールされた評価（「スケジュールされたセグメント化」とも呼ばれます）を有効にする必要があります。 スケジュール済みセグメントを使用すると、IMS組織は繰り返しのスケジュールに従って、セグメントを評価するために書き出しジョブを自動的に実行できます。

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

これで、新しいセグメントと既存のセグメントの両方をストリーミングセグメント用に有効にし、スケジュール済みのセグメントを有効にしてベースラインを作成し、定期的な評価を実行できるようになりました。

Adobe Experience Platformのユーザーインターフェイスを使用して、同様の操作を実行し、セグメントを操作する方法については、 [セグメントビルダーユーザーガイドを参照してください](../ui/overview.md)。