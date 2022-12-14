---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；セグメント化サービス；エッジセグメント化；エッジセグメント化；ストリーミングエッジ；
solution: Experience Platform
title: API を使用したエッジのセグメント化
topic-legacy: developer guide
description: このドキュメントでは、Adobe Experience Platform Segmentation Service API でエッジのセグメント化を使用する方法の例を示します。
exl-id: effce253-3d9b-43ab-b330-943fb196180f
source-git-commit: 8c7c1273feb2033bf338f7669a9b30d9459509f7
workflow-type: tm+mt
source-wordcount: '1187'
ht-degree: 5%

---

# エッジのセグメント化

>[!NOTE]
>
>次のドキュメントでは、API を使用してエッジのセグメント化を実行する方法を説明します。 UI を使用してエッジセグメント化を実行する方法については、 [エッジセグメント化 UI ガイド](../ui/edge-segmentation.md).
>
>エッジセグメント化は、すべての Platform ユーザーが一般に使用できるようになりました。 ベータ版でエッジセグメントを作成した場合、これらのセグメントは引き続き動作します。

エッジのセグメント化は、エッジ上でAdobe Experience Platformで瞬時にセグメントを評価する機能で、同じページや次のページのパーソナライゼーションの使用例を可能にします。

>[!IMPORTANT]
>
> エッジデータは、収集された場所に最も近いエッジサーバーの場所に保存され、ハブ（またはプリンシパル）Adobe Experience Platformデータセンターとして指定された場所以外の場所に保存される場合があります。
>
> さらに、エッジセグメントエンジンは、があるエッジでのリクエストにのみ従います **1 つ** エッジベース以外のプライマリ ID と一致する、プライマリマーク ID。

## はじめに

この開発者ガイドでは、 [!DNL Adobe Experience Platform] エッジのセグメント化に関連するサービス。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合された消費者プロファイルをリアルタイムで提供します。
- [[!DNL Segmentation]](../home.md):セグメントやオーディエンスを [!DNL Real-time Customer Profile] データ。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

任意のExperience PlatformAPI エンドポイントを正しく呼び出すには、 [Platform API の概要](../../landing/api-guide.md) 必要なヘッダーとサンプル API 呼び出しの読み取り方法について説明します。

## エッジセグメント化のクエリタイプ {#query-types}

エッジセグメント化を使用してセグメントを評価するには、クエリを次のガイドラインに従う必要があります。

| クエリタイプ | 詳細 | 例 | PQL の例 |
| ---------- | ------- | ------- | ----------- |
| 単一イベント | 時間制限のない単一の受信イベントを参照するセグメント定義。 | 買い物かごに項目を追加した担当者。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart")])` |
| 単一のプロファイル | 単一のプロファイルのみの属性を参照するセグメント定義 | 米国に住む人々。 | `homeAddress.countryCode = "US"` |
| プロファイルを参照する単一イベント | 時間制限のない 1 つ以上のプロファイル属性と 1 つの受信イベントを参照するセグメント定義。 | ホームページを訪問した米国在住の人。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart")])` |
| プロファイル属性を持つ否定された単一イベント | 否定された単一の受信イベントと 1 つ以上のプロファイル属性を参照するセグメント定義 | 米国に住み、 **not** ホームページにアクセスしました。 | `not(chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView")]))` |
| 1 つの時間枠内の単一イベント | 設定された期間内の単一の受信イベントを参照するセグメント定義。 | 過去 24 時間にホームページを訪問した人。 | `chain(xEvent, timestamp, [X: WHAT(eventType = "addToCart") WHEN(< 8 days before now)])` |
| 時間枠内のプロファイル属性を持つ単一イベント | 1 つ以上のプロファイル属性と、設定された期間内の単一の受信イベントを参照するセグメント定義。 | 過去 24 時間にホームページを訪問した米国に住む人。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [X: WHAT(eventType = "addToCart") WHEN(< 8 days before now)])` |
| 時間枠内のプロファイル属性を持つ 1 つのイベントを無効化 | 1 つ以上のプロファイル属性と、一定期間内の無効な単一の受信イベントを参照するセグメント定義。 | 米国に住み、 **not** 過去 24 時間にホームページにアクセスした。 | `homeAddress.countryCode = "US" and not(chain(xEvent, timestamp, [X: WHAT(eventType = "addToCart") WHEN(< 8 days before now)]))` |
| 24 時間以内の頻度イベント | 24 時間の期間内に特定の回数だけ発生したイベントを参照するセグメント定義。 | ホームページを訪問した人 **少なくとも** 過去 24 時間で 5 回 | `chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] )` |
| 24 時間の時間枠内にプロファイル属性を持つ頻度イベント | 1 つ以上のプロファイル属性と、24 時間の期間内に一定の回数だけ発生したイベントを参照するセグメント定義。 | ホームページを訪問した米国出身の人 **少なくとも** 過去 24 時間で 5 回 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] )` |
| 24 時間以内のプロファイルを含む無効な頻度イベント | 1 つ以上のプロファイル属性と、24 時間の期間内に特定の回数だけ発生する無効なイベントを参照するセグメント定義。 | ホームページを訪問していない人 **詳細** 過去 24 時間で 5 回以上 | `not(chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] ))` |
| 24 時間以内に複数の受信ヒット | 24 時間以内に発生した複数のイベントを参照するセグメント定義。 | ホームページを訪問した人 **または** 過去 24 時間以内にチェックアウトページにアクセスしました。 | `chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 24 hours before now)]) and chain(xEvent, timestamp, [X: WHAT(eventType = "checkoutPageView") WHEN(< 24 hours before now)])` |
| 24 時間の期間内にプロファイルを持つ複数のイベント | 24 時間以内に発生する 1 つ以上のプロファイル属性と複数のイベントを参照するセグメント定義。 | ホームページを訪問した米国出身の人 **および** 過去 24 時間以内にチェックアウトページにアクセスしました。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 24 hours before now)]) and chain(xEvent, timestamp, [X: WHAT(eventType = "checkoutPageView") WHEN(< 24 hours before now)])` |
| セグメントのセグメント | 1 つ以上のバッチセグメントまたはストリーミングセグメントを含むセグメント定義。 | 米国に住んでいて、セグメント「既存のセグメント」に属している人。 | `homeAddress.countryCode = "US" and inSegment("existing segment")` |
| マップを参照するクエリ | プロパティのマップを参照するセグメント定義。 | 外部セグメントデータに基づいて買い物かごに追加した人。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart") WHERE(externalSegmentMapProperty.values().exists(stringProperty="active"))])` |

また、 **必須** エッジ上でアクティブな結合ポリシーに結び付けられている。 結合ポリシーの詳細については、 [結合ポリシーガイド](../../profile/api/merge-policies.md).

セグメント定義は次のようになります。 **not** は、次のシナリオでエッジセグメント化に対して有効になっています。

- セグメント定義には、単一のイベントと `inSegment` イベント。
   - ただし、セグメントが `inSegment` イベントはプロファイルのみ、セグメント定義 **遺言** をエッジセグメント化に対して有効にする。

## エッジセグメント化で有効なすべてのセグメントの取得

IMS 組織内のエッジセグメント化で有効になっているすべてのセグメントのリストを取得するには、にGETリクエストを実行します。 `/segment/definitions` endpoint.

**API 形式**

エッジセグメント化が有効なセグメントを取得するには、クエリパラメーターを含める必要があります `evaluationInfo.synchronous.enabled=true` リクエストパス内で使用します。

```http
GET /segment/definitions?evaluationInfo.synchronous.enabled=true
```

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/segment/definitions?evaluationInfo.synchronous.enabled=true' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、エッジセグメント化が有効な IMS 組織内のセグメントの配列を返します。 返されるセグメント定義の詳細については、 [セグメント定義エンドポイントガイド](./segment-definitions.md).

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
                    "enabled": false
                },
                "synchronous": {
                    "enabled": true
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
                    "enabled": false
                },
                "continuous": {
                    "enabled": false
                },
                "synchronous": {
                    "enabled": true
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

## エッジセグメント化が有効なセグメントの作成

エッジセグメント化を有効にするセグメントを作成するには、 `/segment/definitions` 次のいずれかと一致するエンドポイント [上記のエッジセグメントクエリタイプ](#query-types).

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

>[!NOTE]
>
>次の例は、セグメントを作成するための標準的なリクエストです。 セグメント定義の作成の詳細については、 [セグメントの作成](../tutorials/create-a-segment.md).

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
    }
}'
```

**応答**

正常な応答は、エッジセグメント化に対して有効な、新しく作成されたセグメント定義の詳細を返します。

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
        "value": "chain(xEvent, timestamp, [X: WHAT(var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = "true")])"
    },
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": false
        },
        "synchronous": {
            "enabled": true
        }
    },
    "creationTime": 1572021085000,
    "updateEpoch": 1572021086000,
    "updateTime": 1572021086000
}
```

## 次の手順

エッジセグメント化が有効なセグメントの作成方法がわかったので、それらを使用して、同じページと次のページのパーソナライゼーションの使用例を有効にできます。

Adobe Experience Platform ユーザーインターフェイスを使用して同様のアクションを実行し、セグメントを操作する方法については、『[セグメントビルダーユーザーガイド](../ui/segment-builder.md)』を参照してください。

## 付録

次の節では、エッジセグメント化に関するよくある質問を示します。

### Edge ネットワーク上でセグメントが使用可能になるまで、どれくらいかかりますか？

Edge ネットワーク上でセグメントが使用可能になるまで、最大 1 時間かかります。