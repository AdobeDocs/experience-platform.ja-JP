---
solution: Experience Platform
title: ストリーミングセグメント化を使用してほぼリアルタイムでイベントを評価する
description: このドキュメントでは、Adobe Experience Platform Segmentation Service API でストリーミングセグメント化を使用する方法の例を示します。
role: Developer
exl-id: 119508bd-5b2e-44ce-8ebf-7aef196abd7a
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '1962'
ht-degree: 70%

---

# ストリーミングセグメント化を使用してほぼリアルタイムでイベントを評価する

>[!NOTE]
>
>次のドキュメントでは、API を使用したストリーミングセグメント化の使用方法について説明します。UI を使用したストリーミングセグメント化の使用について詳しくは、[ストリーミングセグメント化 UI ガイド](../ui/streaming-segmentation.md)を参照してください。

[!DNL Adobe Experience Platform] でのストリーミングセグメント化では、データの豊富さに重点を置きながら、ほぼリアルタイムでセグメント化を実行できます。ストリーミングセグメント化を使用すると、ストリーミングデータが [!DNL Platform] に到着する際にセグメントの選定が行われるので、セグメント化ジョブをスケジュールして実行する必要がなくなります。 この機能により、データが [!DNL Platform] に渡される際に、ほとんどのセグメントルールが評価されるようになりました。つまり、スケジュールに沿ったセグメント化ジョブを実行しなくても、セグメントメンバーシップが常に最新の状態に保たれます。

![](../images/api/streaming-segment-evaluation.png)

>[!NOTE]
>
>ストリーミングセグメント化は、ストリーミングソースを使用して取り込まれたすべてのデータに対して機能します。 バッチベースのソースを使用して取り込まれたセグメントは、ストリーミングセグメント化の対象となっている場合でも、毎晩評価されます。
>
>また、ストリーミングセグメント化で評価されたセグメント定義は、バッチセグメント化を使用して評価される別のセグメント定義に基づいている場合、理想的なメンバーシップと実際のメンバーシップの間でずれる可能性があります。 例えば、セグメント A がセグメント B に基づいており、セグメント B がバッチセグメント化を使用して評価される場合、セグメント B は 24 時間ごとにのみ更新されるので、セグメント A はセグメント B の更新と再同期するまで、実際のデータからさらに乖離します。

## はじめに

この開発者ガイドでは、ストリーミングセグメント化に関連する様々な [!DNL Adobe Experience Platform] サービスについて、実際に理解している必要があります。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Segmentation]](../home.md): [!DNL Real-Time Customer Profile] データ。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

以下の節では、[!DNL Platform] API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

この開発者ガイドは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

- Content-Type: application/json

特定のリクエストを完了するには、追加のヘッダーが必要な場合があります。このドキュメントの各例では、正しいヘッダーが表示されます。必要なヘッダーがすべて含まれるように、リクエスト例に特に注意してください。

### ストリーミングセグメント化が有効なクエリタイプ {#query-types}

>[!NOTE]
>
>ストリーミングセグメント化を機能させるには、組織でスケジュールに沿ったセグメント化を有効にする必要があります。スケジュールに沿ったセグメント化を有効にする方法については、[スケジュール済みセグメント化セクションを有効化](#enable-scheduled-segmentation)で確認することができます。

ストリーミングセグメント化を使用してセグメント定義を評価するには、クエリを次のガイドラインに従う必要があります。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 単一イベント | 時間制限のない 1 つの受信イベントを参照するセグメント定義。 |
| 相対時間枠内の単一のイベント | 1 つの受信イベントを参照するセグメント定義。 |
| 時間枠を含む単一イベント | 時間枠を持つ 1 つの受信イベントを参照するセグメント定義。 |
| プロファイルのみ | プロファイル属性のみを参照するセグメント定義。 |
| 24 時間未満の相対時間枠内のプロファイル属性を持つ単一イベント | 1 つ以上のプロファイル属性を持つ単一の受信イベントを参照するセグメント定義で、24 時間未満の相対時間枠内に発生します。 |
| セグメントのセグメント | 1 つ以上のバッチセグメントまたはストリーミングセグメントを含むセグメント定義。 **メモ：**&#x200B;セグメントのセグメントが使用される場合、**24 時間ごとに**&#x200B;プロファイルの不適合が発生します。 |
| プロファイル属性を持つ複数のイベント | **過去 24 時間以内に**&#x200B;複数のイベントを参照し、（オプションで）1 つ以上のプロファイル属性を持つ任意のセグメント定義。 |

次のシナリオでは、セグメント定義はストリーミングセグメント化に対して有効に&#x200B;**なりません**。

- セグメント定義には、Adobe Audience Manager（AAM）のセグメントまたは特性が含まれます。
- セグメント定義には複数のエンティティ（複数エンティティクエリ）が含まれます。
- セグメント定義には、単一のイベントと `inSegment` イベントの組み合わせが含まれます。
   - ただし、`inSegment` イベントに含まれるセグメントがプロファイルのみの場合、セグメント定義はストリーミングセグメント化に対して有効に&#x200B;**なります**。

ストリーミングセグメント化を行う場合は、次のガイドラインに従ってください。

| クエリタイプ | ガイドライン |
| ---------- | -------- |
| 単一イベントクエリ | ルックバックウィンドウに制限はありません。 |
| イベント履歴を含むクエリ | <ul><li>ルックバックウィンドウは **1 日**&#x200B;に制限されています。</li><li>イベント間に厳密な時間順序条件が存在する&#x200B;**必要があります**。</li><li>少なくとも 1 つの否定イベントを含むクエリがサポートされています。 ただし、イベント全体を否定することは&#x200B;**できません**。</li></ul> |

セグメント定義を変更して、ストリーミングセグメント化の条件を満たさなくなった場合、セグメント定義は自動的に「ストリーミング」から「バッチ」に切り替わります。

また、セグメントの選定解除は、セグメントの選定と同様に、リアルタイムで発生します。その結果、プロファイルがセグメント定義の対象にならなくなった場合、そのプロファイルはただちに非認定になります。 例えば、セグメント定義で「過去 3 時間で赤い靴を購入したすべてのユーザー」という要求があった場合、3 時間後に、最初にセグメント定義で選定されたすべてのプロファイルが無効になります。

## ストリーミングセグメント化に対して有効なすべてのセグメント定義を取得

組織内でストリーミングセグメント化が有効になっているすべてのセグメント定義のリストを取得するには、 `/segment/definitions` endpoint.

**API 形式**

ストリーミングが有効なセグメント定義を取得するには、クエリパラメーターを含める必要があります `evaluationInfo.continuous.enabled=true` リクエストパス内で使用します。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、組織内でストリーミングセグメント化が有効なセグメント定義の配列を返します。

```json
{
    "segments": [
        {
            "id": "15063cb-2da8-4851-a2e2-bf59ddd2f004",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
            "imsOrgId": "{ORG_ID}",
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
            "imsOrgId": "{ORG_ID}",
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

## ストリーミングが有効なセグメント定義の作成

セグメント定義が [上記のストリーミングセグメント化タイプ](#query-types).

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

>[!NOTE]
>
>これは、標準の「セグメント定義の作成」リクエストです。 セグメント定義の作成の詳細については、 [セグメント定義の作成](../tutorials/create-a-segment.md).

**応答** 

正常な応答は、新たに作成されたストリーミング対応セグメント定義の詳細を返します。

```json
{
    "id": "f15063cb-2da8-4851-a2e2-bf59ddd2f004",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 30,
    "imsOrgId": "{ORG_ID}",
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

ストリーミング評価を有効にしたら、ベースラインを作成する必要があります（ベースラインの作成後、セグメント定義は常に最新の状態になります）。 システムが自動的にベースライン化を実行するには、まず、スケジュールされた評価（スケジュールされたセグメント化とも呼ばれます）を最初に有効にする必要があります。スケジュールに沿ったセグメント化を使用すると、繰り返しスケジュールに従って、書き出しジョブを自動的に実行し、セグメント定義を評価できます。

>[!NOTE]
>
>[!DNL XDM Individual Profile] の最大 5 つの結合ポリシーを備えたサンドボックスに対して、スケジュールされた評価を有効にできます。組織で、1 つのサンドボックス環境内に [!DNL XDM Individual Profile] の結合ポリシーが 6 つ以上ある場合は、スケジュールされた評価を使用できません。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `properties.segments` | **( 必須 `type` 次と等しい `batch_segmentation`)** 使用 `["*"]` は、すべてのセグメント定義が含まれるようにします。 |
| `schedule` | **（必須）**&#x200B;ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。例（`0 0 1 * * ?`）は、ジョブが毎日 1:00:00 UTC にトリガーされることを意味します。詳しくは、セグメント化のスケジュールのドキュメント内で、[cron 式形式](./schedules.md#appendix)に関する付録を確認してください。 |
| `state` | *（オプション）*&#x200B;スケジュールの状態を含む文字列。使用可能な値は `active` と `inactive` です。デフォルト値は `inactive` です。1 つの組織が作成できるスケジュールは 1 つだけです。 スケジュールを更新する手順は、このチュートリアルの後半で説明します。 |

**応答** 

成功した応答は、新しく作成したスケジュールの詳細を返します。

```json
{
    "id": "cd585edf-962d-420d-94ad-3be03e619ac2",
    "imsOrgId": "{ORG_ID}",
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

次のリクエストでは、スケジュールの `state` を `active` に更新するために、[JSON パッチの形式](https://datatracker.ietf.org/doc/html/rfc6902)を使用しています。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/schedules/cd585edf-962d-420d-94ad-3be03e619ac2 \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

これで、ストリーミングセグメント化の新しいセグメント定義と既存のセグメント定義の両方を有効にし、スケジュールされたセグメント化を有効にして、ベースラインを作成し、定期的な評価を実行できるようになりました。

Adobe Experience Platformユーザーインターフェイスを使用して同様のアクションを実行し、セグメント定義を操作する方法については、 [セグメントビルダーユーザーガイド](../ui/segment-builder.md).

## 付録

次の節では、ストリーミングセグメント化に関するよくある質問を一覧表示しています。

### ストリーミングセグメント化の「選定解除」もリアルタイムで発生しますか。

ほとんどのインスタンスでは、ストリーミングセグメント化の選定解除はリアルタイムで発生します。ただし、セグメントのセグメントを使用するストリーミングセグメント定義では、 **not** はリアルタイムで未認定で、24 時間後に不適格と見なされます。

### ストリーミングセグメント化はどのデータで機能しますか。

ストリーミングセグメント化は、ストリーミングソースを使用して取り込まれたすべてのデータに対して機能します。 バッチベースのソースを使用して取り込まれたセグメントは、ストリーミングセグメント化の対象となっている場合でも、毎晩評価されます。タイムスタンプが 24 時間より古いシステムにストリーミングされたイベントは、後続のバッチジョブで処理されます。

### セグメント定義は、バッチまたはストリーミングによるセグメント化としてどのように定義されますか。

セグメント定義は、クエリタイプとイベント履歴期間の組み合わせに基づいて、バッチまたはストリーミングによるセグメント化として定義されます。 ストリーミングセグメントとして評価されるセグメント定義のリストは、 [ストリーミングセグメント化のクエリタイプの節](#query-types).

セグメントに `inSegment` 式と直接の単一イベントチェーンの&#x200B;**両方**&#x200B;が含まれる場合、ストリーミングセグメント化の対象にはなりません。このセグメント定義をストリーミングセグメント化の対象として認定する場合は、直接の単一イベントチェーンを独自のセグメント定義にする必要があります。

### 「過去 X 日間」の下の数がセグメント定義の詳細セクション内でゼロのままの間、「合計認定済み」セグメント定義の数が増加し続けるのはなぜですか？

合計認定セグメント定義の数は、日別のセグメント化ジョブから取得されます。このジョブには、バッチセグメント定義とストリーミングセグメント定義の両方に適合するオーディエンスが含まれます。 この値は、バッチセグメント定義とストリーミングセグメント定義の両方で表示されます。

「過去 X 日間」の下にある数値は、ストリーミングセグメント化の対象となるオーディエンスの数&#x200B;**のみ**&#x200B;を表し、データをシステムにストリーミングしそのストリーミング定義に照合してカウントする場合に&#x200B;**のみ**&#x200B;増加します。この値は、 **のみ** ストリーミングセグメントの定義に関する説明を表示します。 その結果、この値 **may** バッチセグメント定義の場合は 0 と表示されます。

その結果、「過去 X 日間」の下の数がゼロで、線グラフもゼロをレポートしている場合は、 **not** は、任意のプロファイルを、そのセグメント定義に該当するシステムにストリーミングしました。

### セグメント定義が使用可能になるまで、どれくらいかかりますか。

セグメント定義が使用可能になるまでに最大 1 時間かかります。
