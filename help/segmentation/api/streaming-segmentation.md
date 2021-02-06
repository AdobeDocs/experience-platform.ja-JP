---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；ストリーミングセグメント化；ストリーミングセグメント化；連続評価；
solution: Experience Platform
title: 'ストリーミングセグメント化によりほぼリアルタイムでイベントを評価 '
topic: developer guide
description: このドキュメントでは、Adobe Experience PlatformセグメントサービスAPIを使用したストリーミングセグメントの使用方法の例を示します。
translation-type: tm+mt
source-git-commit: b3defc3e33a55855e307ab70b9797d985d5719e3
workflow-type: tm+mt
source-wordcount: '1338'
ht-degree: 43%

---


# ストリーミングセグメント化により、ほぼリアルタイムでイベントを評価

>[!NOTE]
>
>次のドキュメントでは、APIを使用したストリーミングセグメントの使用方法を説明します。 UIを使用したストリーミングセグメントの使用については、[ストリーミングセグメント化UIガイド](../ui/streaming-segmentation.md)を参照してください。

[!DNL Adobe Experience Platform]のセグメント化をストリーミングすると、お客様はデータの豊富さに重点を置きながら、ほぼリアルタイムでセグメント化を行うことができます。 ストリーミングセグメント化では、セグメント化ジョブのスケジュールや実行の必要性が軽減され、データのストリーミングが[!DNL Platform]に到着する際にセグメントの認定が行われるようになりました。 この機能を使用すると、ほとんどのセグメントルールを[!DNL Platform]に渡すデータと同時に評価できるようになりました。つまり、スケジュール済みのセグメントジョブを実行せずに、セグメントのメンバーシップが最新の状態に維持されます。

![](../images/api/streaming-segment-evaluation.png)

>[!NOTE]
>
>ストリーミングセグメントは、プラットフォームにストリーミングされるデータを評価する場合にのみ使用できます。 つまり、バッチ取り込みによって取り込まれるデータは、ストリーミングセグメント化によって評価されず、夜間にスケジュールされたセグメント化ジョブと共に評価されます。

## はじめに

この開発者ガイドでは、ストリーミングセグメント化に関連する様々な[!DNL Adobe Experience Platform]サービスについて、十分に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統一された消費者プロファイルを提供します。
- [[!DNL Segmentation]](../home.md):デー [!DNL Real-time Customer Profile] タからセグメントやオーディエンスを作成できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 編成する際に使用される標準化されたフレームワーク。

以下の節では、[!DNL Platform] APIを正しく呼び出すために知っておく必要のある追加情報を紹介します。

### API 呼び出し例の読み取り

この開発者ガイドは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

特定のリクエストを完了するには、追加のヘッダーが必要な場合があります。このドキュメントの各例では、正しいヘッダーが表示されます。必要なヘッダーがすべて含まれるように、リクエスト例に特に注意してください。

### ストリーミングセグメント化が有効なクエリタイプ {#streaming-segmentation-query-types}

>[!NOTE]
>
>ストリーミングセグメントを機能させるには、組織でスケジュールされたセグメント化を有効にする必要があります。 スケジュール済みセグメントを有効にする方法については、[「スケジュール済みセグメントを有効にする」セクション](#enable-scheduled-segmentation)を参照してください。

ストリーミングセグメントを使用してセグメントを評価するには、クエリが次のガイドラインに従う必要があります。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 受信ヒット | 時間制限のない、単一の着信イベントを参照するセグメント定義。 |
| 相対時間枠内での着信ヒット | 単一の着信イベントを参照するセグメント定義。 |
| プロファイルのみ | プロファイル属性のみを参照するセグメント定義。 |
| プロファイルを参照する着信ヒット | 時間制限のない、1つの着信イベント、および1つ以上のプロファイル属性を参照するセグメント定義。 |
| 相対的な時間枠内のプロファイルを参照する着信ヒット | 1つの受信イベントと1つ以上のプロファイル属性を参照するセグメント定義。 |
| プロファイルを参照する複数のイベント | 過去24時間以内に複数のイベント&#x200B;**を参照するセグメント定義には、1つ以上のプロファイル属性が含まれます。**（オプション） |

次のシナリオでは、セグメント定義はストリーミングセグメントに対して&#x200B;**有効になりません**。

- セグメント定義には、Adobe Audience Manager(AAM)のセグメントまたは特性が含まれます。
- セグメント定義には、複数のエンティティ(マルチエンティティクエリ)が含まれます。

さらに、ストリーミングセグメント化を行う際には、次のようなガイドラインが適用されます。

| クエリタイプ | ガイドライン |
| ---------- | -------- |
| 単一イベントクエリ | ルックバックウィンドウに制限はありません。 |
| イベント履歴のあるクエリ | <ul><li>ルックバックウィンドウは、**1日**&#x200B;に制限されます。</li><li>厳密な時間順序条件&#x200B;**は、イベント間に**&#x200B;が存在する必要があります。</li><li>少なくとも1つの無効なイベントを持つクエリがサポートされます。 ただし、イベント&#x200B;**全体を否定にすることはできません。**</li></ul> |

## ストリーミングセグメントで有効になっているすべてのセグメントを取得

`/segment/definitions`エンドポイントにGETリクエストを行うことで、IMS組織内でのストリーミングセグメント化が有効になっているすべてのセグメントのリストを取得できます。

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

](#streaming-segmentation-query-types)に示した[ストリーミングセグメントタイプのいずれかと一致する場合、セグメントは自動的にストリーミングが有効になります。

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
>これは、標準の「セグメントの作成」リクエストです。 セグメント定義の作成についての詳細は、[セグメント](../tutorials/create-a-segment.md)の作成のチュートリアルをお読みください。

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
>[!DNL XDM Individual Profile]に対して、最大5個のマージポリシーを持つサンドボックスに対して、スケジュールされた評価を有効にできます。 1つのサンドボックス環境内に[!DNL XDM Individual Profile]のマージポリシーが5つを超える場合、スケジュールされた評価を使用できません。

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