---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミングセグメント化
topic: developer guide
translation-type: tm+mt
source-git-commit: e351a2d489730c1f1bd5f87be8d85612090bc009
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 43%

---


# ストリーミングセグメント化により、ほぼリアルタイムでイベントを評価

>[!NOTE]
>
>次のドキュメントでは、APIを使用したストリーミングセグメントの使用方法を説明します。 UIを使用したストリーミングセグメントの使用について詳しくは、『ストリー [ミングセグメント化UI』ガイドを参照してください](../ui/streaming-segmentation.md)。

セグメント化のストリーミング [!DNL Adobe Experience Platform] により、お客様はデータの豊富性に重点を置きながら、ほぼリアルタイムでセグメント化を行うことができます。 ストリーミングセグメント化では、データが到着する際にセグメントの認定が行われ、セグメント化ジョブのスケジュール [!DNL Platform]や実行の必要性が軽減されました。 With this capability, most segment rules can now be evaluated as the data is passed into [!DNL Platform], meaning segment membership will be kept up-to-date without running scheduled segmentation jobs.

![](../images/api/streaming-segment-evaluation.png)

## はじめに

This developer guide requires a working understanding of the various [!DNL Adobe Experience Platform] services involved with streaming segmentation. このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統一された消費者プロファイルを提供します。
- [!DNL Segmentation](../home.md): データからセグメントやオーディエンスを作成する機能を提供し [!DNL Real-time Customer Profile] ます。
- [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

The following sections provide additional information that you will need to know in order to successfully make calls to [!DNL Platform] APIs.

### API 呼び出し例の読み取り

この開発者ガイドは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

特定のリクエストを完了するには、追加のヘッダーが必要な場合があります。このドキュメントの各例では、正しいヘッダーが表示されます。必要なヘッダーがすべて含まれるように、リクエスト例に特に注意してください。

### ストリーミングセグメント化が有効なクエリタイプ {#streaming-segmentation-query-types}

>[!NOTE]
>
>ストリーミングセグメントを機能させるには、組織でスケジュールされたセグメント化を有効にする必要があります。 スケジュール済みセグメントを有効にする方法については、「スケジュール済みセグメントを [有効にする」の節を参照してください。](#enable-scheduled-segmentation)

ストリーミングセグメントを使用してセグメントを評価するには、クエリが次のガイドラインに従う必要があります。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 受信ヒット | 時間制限のない、単一の着信イベントを参照するセグメント定義。 |
| 相対時間枠内での着信ヒット | 過去7日間に発生した単一のイベントを参照す **るセグメント定義**。 |
| プロファイルを参照する着信ヒット | 時間制限のない、1つの着信イベント、および1つ以上のプロファイル属性を参照するセグメント定義。 |
| 相対的な時間枠内のプロファイルを参照する着信ヒット | 過去7日間の、1つの着信イベントと1つ以上のプロファイル属性を参照す **るセグメント定義**。 |
| プロファイルを参照する複数のイベント | 過去24時間以内に複数のイベントを参照するセグメント定義 **には** 、1つ以上のプロファイル属性が含まれます。 |

次の節では、ストリーミングセグメントに対して **有効にしないリストセグメント定義の例を示します** 。

| クエリタイプ | 詳細 |
| ---------- | ------- | 
| 相対時間枠内での着信ヒット | セグメント定義が、 **過去7日間** 以内でない着信イベントを参照する場合 ****。 例えば、 **過去2週間以内の場合**。 |
| 相対的なウィンドウ内のプロファイルを参照する着信ヒット | 次のオプションは、ストリーミングセグメントをサポートし **ません** 。<ul><li>過去7日間 **以** 内の着信イベント ****。</li><li>Adobe Audience Manager(AAM)のセグメントまたは特性を含むセグメント定義。</li></ul> |
| プロファイルを参照する複数のイベント | 次のオプションは、ストリーミングセグメントをサポートし **ません** 。<ul><li>過去24時間以内に **発生しないイベント******。</li><li>Adobe Audience Manager(AAM)のセグメントまたは特性を含むセグメント定義。</li></ul> |
| マルチエンティティクエリ | マルチエンティティクエリは、全体として、ストリーミングセグメントでは **サポートされていません** 。 |

さらに、ストリーミングセグメント化を行う際には、次のようなガイドラインが適用されます。

| クエリタイプ | ガイドライン |
| ---------- | -------- |
| 単一イベントクエリ | ルックバックウィンドウは **7日間に制限されています**。 |
| イベント履歴のあるクエリ | <ul><li>ルックバックウィンドウは **1日に制限されます**。</li><li>イベント間に厳密な時間順序条件 **が存在する** 。</li><li>イベント間の単純な時間順（前後）のみが許可されます。</li><li>個々のイベント **を無効にすることはできません** 。 ただし、クエリ全体を無効にす **ることはできます** 。</li></ul> |

## ストリーミングセグメントで有効になっているすべてのセグメントを取得

エンドポイントにGETリクエストを行うことで、IMS組織内でストリーミングセグメント化が有効になっているすべてのセグメントのリストを取得でき `/segment/definitions` ます。

**API 形式**

ストリーミングが対応セグメントを取得するには、リクエストパスに `evaluationInfo.continuous.enabled=true` クエリパラメーターを含める必要があります。

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

正常な応答は、ストリーミングセグメント化対応 IMS 組織内セグメントの配列を返します。

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

## ストリーミング対応セグメントの作成

上記のいずれかのストリー [ミングセグメントタイプと一致する場合、セグメントは自動的にストリーミングが有効になります](#streaming-segmentation-query-types)。

**API 形式**

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

>[!NOTE]
>
>これは、標準の「セグメントの作成」リクエストです。 For more information about creating a segment definition, please read the tutorial on [creating a segment](../tutorials/create-a-segment.md).

**応答** 

正常な応答は、新たに作成されたストリーミング対応セグメント定義の詳細を返します。

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

ストリーミング評価が有効になったら、ベースラインを作成する必要があります（ベースラインの作成後、セグメントは常に最新の状態になります）。システムが自動的にベースライン設定を実行するには、スケジュールされた評価（「スケジュールされたセグメント化」とも呼ばれます）を有効にする必要があります。 スケジュール済みセグメントを使用すると、IMS組織は繰り返しのスケジュールに従って、セグメントを評価するために書き出しジョブを自動的に実行できます。

>[!NOTE]
>
>Scheduled evaluation can be enabled for sandboxes with a maximum of five (5) merge policies for [!DNL XDM Individual Profile]. If your organization has more than five merge policies for [!DNL XDM Individual Profile] within a single sandbox environment, you will not be able to use scheduled evaluation.

### スケジュールの作成

`/config/schedules` エンドポイントに対して POST リクエストを実行することにより、スケジュールを作成し、スケジュールをトリガーする必要がある特定の時間を指定することができます。

**API 形式**

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
| `name` | **（必須）**&#x200B;スケジュールの名前。文字列である必要があります。 |
| `type` | **（必須）**&#x200B;文字列形式のジョブタイプ。サポートされているタイプは `batch_segmentation` と `export` です。 |
| `properties` | **（必須）**&#x200B;スケジュールに関連する追加のプロパティを含むオブジェクト。 |
| `properties.segments` | **（`type`が`batch_segmentation`と等しい場合は必須）**`["*"]` を使用すると、すべてのセグメントが含まれます。 |
| `schedule` | **（必須）**&#x200B;ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。例（`0 0 1 * * ?`）は、ジョブが毎日1:00:00 UTC にトリガーされることを示しています。詳しくは、[CRON 式形式](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)のドキュメントを参照してください。 |
| `state` | *（オプション）*&#x200B;スケジュールの状態を含む文字列。使用可能な値は `active` と `inactive` です。デフォルト値は `inactive` です。IMS 組織で作成できるスケジュールは 1 つだけです。スケジュールを更新する手順は、このチュートリアルの後半で説明します。 |

**応答** 

成功した応答は、新しく作成したスケジュールの詳細を返します。

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

デフォルトでは、作成（POST）リクエスト本文で `state` プロパティが `active` に設定されていない限り、スケジュールは作成時に非アクティブになります。`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、パスにスケジュールの ID を含めることで、スケジュールを有効にする（`state` を `active` に設定する）ことができます。

**API 形式**

```http
POST /config/schedules/{SCHEDULE_ID}
```

**リクエスト**

次のリクエストでは、スケジュールの `state` を `active` に更新するために、[JSON パッチの形式](http://jsonpatch.com/)を使用しています。

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

更新が成功すると、空の応答本文と HTTP ステータス 204（No Content）が返されます。

同じ操作を使用して、前のリクエストの「値」を「inactive」に置き換えることで、スケジュールを無効にできます。

## 次の手順

新しいセグメントと既存のセグメントの両方でストリーミングセグメント化を有効にし、スケジュールされたセグメント化を有効にしてベースラインを開発し、定期評価を実行したので、組織のセグメントの作成を開始できます。

Adobe Experience Platform ユーザーインターフェイスを使用して同様のアクションを実行し、セグメントを操作する方法については、『[セグメントビルダーユーザーガイド](../ui/segment-builder.md)』を参照してください。