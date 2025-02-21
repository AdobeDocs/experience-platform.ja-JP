---
solution: Experience Platform
title: ストリーミングセグメント化ガイド
description: ストリーミングセグメント化の概要、ストリーミングセグメント化を使用して評価されたオーディエンスの作成方法、ストリーミングセグメント化を使用して作成されたオーディエンスの表示方法について説明します。
exl-id: cb9b32ce-7c0f-4477-8c49-7de0fa310b97
source-git-commit: 3f0cfd6c36344f481751bf05236df4fb288eab60
workflow-type: tm+mt
source-wordcount: '1253'
ht-degree: 32%

---

# ストリーミングセグメント化ガイド

ストリーミングセグメント化は、データの豊富さを重視しながら、ほぼリアルタイムでAdobe Experience Platformのオーディエンスを評価する機能です。

ストリーミングセグメント化を使用すると、ストリーミングデータが Platform に到着する際にオーディエンスの選定が行われるので、セグメント化ジョブをスケジュールして実行する必要がなくなります。 これにより、Platform に渡されるデータを評価し、オーディエンスメンバーシップを自動的に最新の状態に保つことができます。

## 適格なクエリタイプ {#query-types}

クエリは、次の表に示す条件のいずれかを満たす場合、ストリーミングセグメント化の対象になります。

>[!NOTE]
>
>ストリーミングセグメント化を機能させるには、スケジュールされたセグメント化を組織で有効にする必要があります。 スケジュールに沿ったセグメント化を有効にする方法について詳しくは、[Audience Portal の概要 ](../ui/audience-portal.md#scheduled-segmentation) を参照してください。

| クエリタイプ | 詳細 | クエリ | 例 |
| ---------- | ------- | ----- | ------- |
| 24 時間未満の時間枠内での単一イベント | 24 時間未満の時間枠内に 1 つの受信イベントを参照する任意のセグメント定義。 | `CHAIN(xEvent, timestamp, [C0: WHAT(eventType.equals("commerce.checkouts", false)) WHEN(today)])` | ![相対時間枠内の単一イベントの例](../images/methods/streaming/single-event.png) |
| プロファイルのみ | プロファイル属性のみを参照するセグメント定義。 | `homeAddress.country.equals("US", false)` | ![ 表示されるプロファイル属性の例 ](../images/methods/streaming/profile-attribute.png) |
| 24 時間未満の相対時間枠内でのプロファイル属性を持つ単一のイベント | 1 つ以上のプロファイル属性を持つ 1 つの受信イベントを参照し、24 時間未満の相対時間枠内に発生するセグメント定義。 | `workAddress.country.equals("US", false) and CHAIN(xEvent, timestamp, [C0: WHAT(eventType.equals("commerce.checkouts", false)) WHEN(today)])` | ![相対時間枠内にプロファイル属性を持つ単一イベントの例](../images/methods/streaming/single-event-with-profile-attribute.png) |
| セグメントのセグメント | 1 つ以上のバッチセグメントまたはストリーミングセグメントを含むセグメント定義。 **メモ：**&#x200B;セグメントのセグメントが使用される場合、**24 時間ごとに**&#x200B;プロファイルの不適合が発生します。 | `inSegment("a730ed3f-119c-415b-a4ac-27c396ae2dff") and inSegment("8fbbe169-2da6-4c9d-a332-b6a6ecf559b9")` | ![セグメントのセグメントの例](../images/methods/streaming/segment-of-segments.png) |
| プロファイル属性を持つ複数のイベント | **過去 24 時間以内に**&#x200B;複数のイベントを参照し、（オプションで）1 つ以上のプロファイル属性を持つ任意のセグメント定義。 | `workAddress.country.equals("US", false) and CHAIN(xEvent, timestamp, [C0: WHAT(eventType.equals("directMarketing.emailClicked", false)) WHEN(today), C1: WHAT(eventType.equals("commerce.checkouts", false)) WHEN(today)])` | ![プロファイル属性を持つ複数イベントの例](../images/methods/streaming/multiple-events-with-profile-attribute.png) |

次のシナリオでは、セグメント定義はストリーミングセグメント化の対象 **なります** 対象外）。

- セグメント定義には、Adobe Audience Manager（AAM）のセグメントまたは特性が含まれます。
- セグメント定義には複数のエンティティ（複数エンティティクエリ）が含まれます。
- セグメント定義には、単一のイベントと `inSegment` イベントの組み合わせが含まれます。
   - ただし、`inSegment` イベントに含まれるセグメント定義がプロファイルのみの場合、セグメント定義はストリーミングセグメント化に対して有効に&#x200B;**なります**。
- セグメント定義では、時間制約の一部として「年を無視」を使用します。

ストリーミングセグメント化クエリに適用される次のガイドラインに注意してください。

| クエリタイプ | ガイドライン |
| ---------- | -------- |
| 単一イベントクエリ | ルックバックウィンドウに制限はありません。 |
| イベント履歴を含むクエリ | <ul><li>ルックバックウィンドウは **1 日**&#x200B;に制限されています。</li><li>イベント間に厳密な時間順序条件が存在する&#x200B;**必要があります**。</li><li>少なくとも 1 つの否定イベントを含むクエリがサポートされています。 ただし、イベント全体を否定することは&#x200B;**できません**。</li></ul> |

セグメント定義を変更して、ストリーミングセグメント化の条件を満たさなくなった場合、セグメント定義は自動的に「ストリーミング」から「バッチ」に切り替わります。

また、セグメントの選定解除は、セグメントの選定と同様に、リアルタイムで発生します。その結果、オーディエンスがセグメントの選定対象ではなくなった場合、そのオーディエンスはただちに選定解除となります。例えば、セグメント定義で「過去 3 時間で赤い靴を購入したすべてのユーザー」という要求があった場合、3 時間後に、最初にセグメント定義で選定されたすべてのプロファイルが無効になります。

## オーディエンスを作成 {#create-audience}

Segmentation Service API または UI のオーディエンスポータルを使用して、ストリーミングセグメント化を使用して評価されるオーディエンスを作成できます。

セグメント定義が [ 適格なクエリタイプ ](#eligible-query-types) のいずれかと一致する場合、ストリーミングを有効にすることができます。

>[!BEGINTABS]

>[!TAB Segmentation Service API]

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

+++ ストリーミングセグメント化が有効なセグメント定義を作成するリクエストの例

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/definitions
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "name": "People in the USA",
        "description: "An audience that looks for people who live in the USA",
        "expression": {
            "type": "PQL",
            "format": "pql/text",
            "value": "homeAddress.country = \"US\""
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
        "schema": {
            "name": "_xdm.context.profile"
        }
     }'
```

+++

**応答**

リクエストが成功した場合は、新しく作成したセグメント定義の詳細と HTTP ステータス 200 が返されます。

+++セグメント定義を作成する際の応答のサンプル

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "profileInstanceId": "ups",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "People in the USA",
    "description": "An audience that looks for people who live in the USA",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "homeAddress.country = \"US\""
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
    "dataGovernancePolicy": {
        "excludeOptOut": true
    },
    "creationTime": 0,
    "updateEpoch": 1579292094,
    "updateTime": 1579292094000
}
```

+++

このエンドポイントの使用について詳しくは、[ セグメント定義エンドポイントガイド ](../api/segment-definitions.md) を参照してください。

>[!TAB  オーディエンスポータル ]

オーディエンスポータルで、「**[!UICONTROL オーディエンスを作成]**」を選択します。

![ オーディエンスを作成ボタンは、オーディエンスポータルでハイライト表示されます。](../images/methods/streaming/select-create-audience.png)

ポップオーバーが表示されます。 **[!UICONTROL ルールを作成]** を選択して、セグメントビルダーに入ります。

![ オーディエンスを作成ポップオーバーで「ルールを作成」ボタンがハイライト表示されます。](../images/methods/streaming/select-build-rules.png)

セグメントビルダー内で、[ 適格なクエリタイプ ](#eligible-query-types) の 1 つに一致するセグメント定義を作成します。 セグメント定義がストリーミングセグメント化の対象になると、**[!UICONTROL 評価方法]** として **[!UICONTROL ストリーミング]** を選択できるようになります。

![ セグメント定義が表示されます。 評価タイプがハイライト表示され、ストリーミングセグメント化を使用してセグメント定義を評価できることが示されています。](../images/methods/streaming/streaming-evaluation-method.png)

セグメント定義の作成について詳しくは、[セグメントビルダーガイド](../ui/segment-builder.md)を参照してください。

>[!ENDTABS]

## オーディエンスの取得 {#retrieve-audiences}

Segmentation Service API または UI のオーディエンスポータルを使用して、ストリーミングセグメント化を使用して評価されるすべてのオーディエンスを取得できます。

>[!BEGINTABS]

>[!TAB Segmentation Service API]

`/segment/definitions` エンドポイントに対してGET リクエストを行い、組織内でストリーミングセグメント化を使用して評価されるすべてのセグメント定義のリストを取得します。

**API 形式**

ストリーミングセグメント化を使用して評価されたセグメント定義を取得するには、リクエストパスにクエリパラメーター `evaluationInfo.synchronous.enabled=true` を含める必要があります。

```http
GET /segment/definitions?evaluationInfo.continuous.enabled=true
```

**リクエスト**

+++ ストリーミングが有効なすべてのセグメント定義を一覧表示するサンプルリクエスト

```shell
curl -X GET 'https://platform.adobe.io/data/core/ups/segment/definitions?evaluationInfo.continuous.enabled=true' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 が、ストリーミングセグメント化が有効になっている組織内のセグメント定義の配列と共に返されます。

+++組織内のストリーミングセグメント化が有効なすべてのセグメント定義のリストを含むサンプル応答

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

返されるセグメント定義について詳しくは、[セグメント定義エンドポイントガイド](../api/segment-definitions.md)を参照してください。

+++

>[!TAB  オーディエンスポータル ]

オーディエンスポータルのフィルターを使用すると、組織内でストリーミングセグメント化が有効になっているすべてのオーディエンスを取得できます。 ![ フィルターアイコン ](../../images/icons/filter.png) アイコンを選択して、フィルターのリストを表示します。

![Audience Portal でフィルターアイコンがハイライト表示されています。](../images/methods/filter-audiences.png)

使用可能なフィルター内で、**[!UICONTROL 頻度を更新]** に移動し、「[!UICONTROL  ストリーミング ]」を選択します。 このフィルターを使用すると、ストリーミングセグメント化を使用して評価された、組織内のすべてのオーディエンスが表示されます。

![ ストリーミングの更新頻度が選択され、ストリーミングセグメント化を使用して評価される組織内のすべてのオーディエンスが表示されます。](../images/methods/streaming/filter-streaming.png)

Platform でのオーディエンスの表示について詳しくは、[ オーディエンスポータルガイド ](../ui/audience-portal.md) を参照してください。

>[!ENDTABS]

## オーディエンスの詳細 {#audience-details}

ストリーミングセグメント化を使用して評価された特定のオーディエンスの詳細を、オーディエンスポータル内で選択して表示できます。

オーディエンスポータルでオーディエンスを選択すると、オーディエンスの詳細ページが表示されます。 オーディエンスの詳細の概要、選定されたプロファイルの量の推移、オーディエンスがアクティブ化されている宛先など、オーディエンスに関する情報が表示されます。

![ ストリーミングセグメント化を使用して評価されたオーディエンスに関するオーディエンスの詳細ページが表示されます。](../images/methods/streaming/audience-details.png)

ストリーミングが有効なオーディエンスの場合は、**[!UICONTROL プロファイルの推移]** カードが表示され、合計選定済み指標と新しいオーディエンスの更新済み指標が表示されます。

**[!UICONTROL 合計選定]** 指標は、このオーディエンスのバッチおよびストリーミングの評価に基づいた、選定オーディエンスの合計数を表します。

**[!UICONTROL 更新された新しいオーディエンス]** 指標は、ストリーミングセグメント化によるオーディエンスサイズの変化を示す折れ線グラフで表されます。 ドロップダウンを調整して、過去 24 時間、先週または過去 30 日間を表示できます。

![ プロファイルの推移カードがハイライト表示されています。](../images/methods/streaming/profiles-over-time.png)

オーディエンスの詳細については、[ オーディエンスポータルの概要 ](../ui/audience-portal.md#audience-details) を参照してください。

## 次の手順

このガイドでは、ストリーミングが有効なセグメント定義がAdobe Experience Platformでどのように機能するか、およびストリーミングが有効なセグメント定義をどのように監視するかについて説明します。

Adobe Experience Platform ユーザーインターフェイスの使用について詳しくは、[セグメント化ユーザーガイド](./overview.md)を参照してください。

ストリーミングセグメント化に関するよくある質問については、[FAQ のストリーミングセグメント化の節 ](../faq.md#streaming-segmentation) を参照してください。
