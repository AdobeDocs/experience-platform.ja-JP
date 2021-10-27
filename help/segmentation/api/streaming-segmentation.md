---
keywords: Experience Platform；ホーム；人気の高いトピック；セグメント化；セグメント化；セグメント化サービス；ストリーミングセグメント化；ストリーミングセグメント化；継続的評価；
solution: Experience Platform
title: 'ストリーミングセグメント化を使用したほぼリアルタイムでのイベント評価 '
topic-legacy: developer guide
description: このドキュメントでは、Adobe Experience Platform Segmentation Service API でストリーミングセグメント化を使用する方法の例を示します。
exl-id: 119508bd-5b2e-44ce-8ebf-7aef196abd7a
source-git-commit: bb5a56557ce162395511ca9a3a2b98726ce6c190
workflow-type: tm+mt
source-wordcount: '1411'
ht-degree: 44%

---

# ストリーミングセグメント化を使用したほぼリアルタイムでのイベント評価

>[!NOTE]
>
>次のドキュメントでは、API を使用したストリーミングセグメント化の使用方法について説明します。 UI を使用したストリーミングセグメント化の使用について詳しくは、 [ストリーミングセグメント化 UI ガイド](../ui/streaming-segmentation.md).

でのストリーミングセグメント化 [!DNL Adobe Experience Platform] では、データの充実性に重点を置きながら、ほぼリアルタイムでセグメント化をおこなうことができます。 ストリーミングセグメント化を使用すると、ストリーミングデータがにランディングされるとセグメント認定がおこなわれるようになりました。 [!DNL Platform]を使用することで、セグメント化ジョブのスケジュールおよび実行の必要性を軽減できます。 この機能を使用すると、ほとんどのセグメントルールを、 [!DNL Platform]と同じです。つまり、セグメントメンバーシップは、スケジュールされたセグメント化ジョブを実行せずに最新の状態に保たれます。

![](../images/api/streaming-segment-evaluation.png)

>[!NOTE]
>
>ストリーミングによるセグメント化は、Platform にストリーミングされるデータを評価する場合にのみ使用できます。 つまり、バッチ取り込みで取り込まれたデータは、ストリーミングのセグメント化によって評価されず、夜間にスケジュールされたセグメント化ジョブと共に評価されます。

## はじめに

この開発者ガイドでは、 [!DNL Adobe Experience Platform] ストリーミングセグメント化に関連するサービス。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合された消費者プロファイルをリアルタイムで提供します。
- [[!DNL Segmentation]](../home.md):セグメントやオーディエンスを [!DNL Real-time Customer Profile] データ。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。

以下の節では、への呼び出しを正しくおこなうために知っておく必要がある追加情報を示します。 [!DNL Platform] API

### API 呼び出し例の読み取り

この開発者ガイドは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

特定のリクエストを完了するには、追加のヘッダーが必要な場合があります。このドキュメントの各例では、正しいヘッダーが表示されます。必要なヘッダーがすべて含まれるように、リクエスト例に特に注意してください。

### ストリーミングセグメント化が有効なクエリタイプ {#streaming-segmentation-query-types}

>[!NOTE]
>
>ストリーミングセグメント化を機能させるには、組織でスケジュールに沿ったセグメント化を有効にする必要があります。 スケジュールに沿ったセグメント化を有効にする方法については、 [スケジュール済みセグメント化セクションを有効化](#enable-scheduled-segmentation)

ストリーミングセグメント化を使用してセグメントを評価するには、クエリを次のガイドラインに従う必要があります。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 単一イベント | 時間制限のない単一の受信イベントを参照するセグメント定義。 |
| 相対時間枠内の単一イベント | 単一の受信イベントを参照するセグメント定義。 |
| 時間枠を含む単一イベント | 時間枠を持つ単一の受信イベントを参照するセグメント定義。 |
| プロファイルのみ | プロファイル属性のみを参照するセグメント定義。 |
| プロファイル属性を持つ単一のイベント | 時間制限のない単一の受信イベントを参照するセグメント定義、1 つ以上のプロファイル属性。 **注意：** クエリは、イベントの発生時に即座に評価されます。 ただし、プロファイルイベントの場合は、組み込まれるまで 24 時間待つ必要があります。 |
| 相対時間枠内のプロファイル属性を持つ単一イベント | 1 つの受信イベントと 1 つ以上のプロファイル属性を参照するセグメント定義。 |
| セグメントのセグメント | 1 つ以上のバッチセグメントまたはストリーミングセグメントを含むセグメント定義。 **注意：** セグメントのセグメントが使用される場合、プロファイルの不適格が発生します **24 時間ごと**. |
| プロファイル属性を持つ複数のイベント | 複数のイベントを参照する任意のセグメント定義 **過去 24 時間以内に** （オプションで）1 つ以上のプロファイル属性を持ちます。 |

セグメント定義は次のようになります。 **not** は、次のシナリオでストリーミングセグメント化に対して有効になります。

- セグメント定義には、Adobe Audience Manager(AAM) のセグメントまたは特性が含まれます。
- セグメント定義には複数のエンティティ（複数エンティティクエリ）が含まれます。

また、ストリーミングセグメント化の際には、次のガイドラインが適用されます。

| クエリタイプ | ガイドライン |
| ---------- | -------- |
| 単一イベントクエリ | ルックバックウィンドウに制限はありません。 |
| イベント履歴を含むクエリ | <ul><li>ルックバックウィンドウは次の条件に制限されています。 **1 日**.</li><li>厳密な時間順序条件 **必須** イベント間に存在する。</li><li>少なくとも 1 つの否定イベントを持つクエリがサポートされています。 ただし、イベント全体 **できません** 否定です。</li></ul> |

## ストリーミングセグメント化が有効なすべてのセグメントを取得

IMS 組織内でストリーミングセグメント化が有効になっているすべてのセグメントのリストを取得するには、GETリクエストを `/segment/definitions` endpoint.

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

セグメントが [上記のストリーミングセグメントタイプ](#streaming-segmentation-query-types).

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
>これは、標準の「セグメントの作成」リクエストです。 セグメント定義の作成の詳細については、 [セグメントの作成](../tutorials/create-a-segment.md).

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

ストリーミング評価が有効になったら、ベースラインを作成する必要があります（ベースラインの作成後、セグメントは常に最新の状態になります）。システムが自動的にベースライン化を実行するには、まず、スケジュールされた評価（スケジュールされたセグメント化とも呼ばれます）を有効にする必要があります。 スケジュールに沿ったセグメント化を使用すると、IMS 組織は定期的なスケジュールに従って、セグメントを評価する書き出しジョブを自動的に実行できます。

>[!NOTE]
>
>の結合ポリシーが最大 5 個あるサンドボックスに対して、スケジュールされた評価を有効にすることができます。 [!DNL XDM Individual Profile]. 組織に対する結合ポリシーが 6 つ以上ある場合 [!DNL XDM Individual Profile] 単一のサンドボックス環境内では、スケジュールされた評価を使用できません。

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
| `properties.segments` | **（`type` が `batch_segmentation` と等しい場合は必須）**`["*"]` を使用すると、すべてのセグメントが含まれます。 |
| `schedule` | **（必須）**&#x200B;ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。例 (`0 0 1 * * ?`) は、ジョブが毎日 1 時にトリガーされることを意味します。:00:00 UTC です。 詳しくは、[CRON 式形式](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)のドキュメントを参照してください。 |
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

これで、新しいセグメントと既存のセグメントの両方を有効にし、スケジュールされたセグメント化を有効にしてベースラインを作成し、定期的な評価を実行できるようになりました。組織で、ストリーミング対応セグメントの作成を開始できます。

Adobe Experience Platform ユーザーインターフェイスを使用して同様のアクションを実行し、セグメントを操作する方法については、『[セグメントビルダーユーザーガイド](../ui/segment-builder.md)』を参照してください。
