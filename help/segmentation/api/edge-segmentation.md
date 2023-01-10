---
keywords: Experience Platform;ホーム;人気のトピック;セグメント化;セグメント化;セグメント化サービス;エッジセグメント化;エッジセグメント化;ストリーミングエッジ;
solution: Experience Platform
title: API を使用したエッジのセグメント化
description: このドキュメントでは、Adobe Experience Platform Segmentation Service API でエッジのセグメント化を使用する方法の例を示します。
exl-id: effce253-3d9b-43ab-b330-943fb196180f
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '1187'
ht-degree: 96%

---

# エッジのセグメント化

>[!NOTE]
>
>次のドキュメントでは、API を使用してエッジのセグメント化を実行する方法を説明します。 UI を使用してエッジのセグメント化を実行する方法については、[エッジセグメント化 UI ガイド](../ui/edge-segmentation.md)を参照してください。
>
>エッジセグメント化は、すべての Platform ユーザーが一般に使用できるようになりました。ベータ版でエッジセグメントを作成した場合、これらのセグメントは引き続き動作します。

エッジのセグメント化は、Adobe Experience Platform のセグメントを Experience Edge 上で瞬時に評価する機能で、同じページや次のページのパーソナライゼーションのユースケースを可能にします。

>[!IMPORTANT]
>
> エッジデータは、収集された場所に最も近いエッジサーバーの場所に保存されるので、ハブ（またはプリンシパル）Adobe Experience Platform データセンターとして指定された場所以外に保存される可能性があります。
>
> さらに、エッジセグメント化エンジンは、プライマリとしてマークされた&#x200B;**単一**&#x200B;の ID（エッジベース以外のプライマリ ID と一致するもの）があるエッジ上のリクエストのみに従います。

## はじめに

この開発者ガイドでは、エッジのセグメント化に関連する様々な [!DNL Adobe Experience Platform] サービスについての十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合された消費者プロファイルをリアルタイムで提供します。
- [[!DNL Segmentation]](../home.md)：[!DNL Real-Time Customer Profile] データからセグメントやオーディエンスを作成できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

Experience Platform API エンドポイントへの呼び出しを正常に行うには、[Platform API の基本を学ぶ](../../landing/api-guide.md)のガイドを読み、必要なヘッダーとサンプル API 呼び出しの読み方を確認してください。

## エッジセグメント化のクエリタイプ {#query-types}

エッジセグメント化を使用してセグメントを評価するには、次のガイドラインに従ってクエリを行う必要があります。

| クエリタイプ | 詳細 | 例 | PQL の例 |
| ---------- | ------- | ------- | ----------- |
| 単一イベント | 時間制限のない 1 つの受信イベントを参照するセグメント定義。 | 買い物かごにアイテムを追加した人物。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart")])` |
| 単一プロファイル | 単一プロファイルのみの属性を参照するセグメント定義 | 米国在住の人物。 | `homeAddress.countryCode = "US"` |
| プロファイルを参照する単一のイベント | 1 つ以上のプロファイル属性と、時間制限のない単一の受信イベントを参照する任意のセグメント定義。 | ホームページを訪問した米国在住の人物。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart")])` |
| プロファイル属性を持つ単一イベントの否定 | 単一の受信イベントの否定と 1 つ以上のプロファイル属性を参照する任意のセグメント定義 | ホームページを訪問&#x200B;**したことがない**&#x200B;米国在住の人物。 | `not(chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView")]))` |
| 時間枠内での単一のイベント | 設定された期間内の単一の受信イベントを参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを訪問した人物。 | `chain(xEvent, timestamp, [X: WHAT(eventType = "addToCart") WHEN(< 8 days before now)])` |
| 時間枠内でのプロファイル属性を持つ単一イベント | 1 つ以上のプロファイル属性と、設定された期間内の単一の受信イベントを参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを訪問した米国在住の人物。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [X: WHAT(eventType = "addToCart") WHEN(< 8 days before now)])` |
| 時間枠内でのプロファイル属性を持つ単一イベントの否定 | 1 つ以上のプロファイル属性と、期間内の単一受信イベントの否定を参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを訪問&#x200B;**していない**&#x200B;米国在住の人物。 | `homeAddress.countryCode = "US" and not(chain(xEvent, timestamp, [X: WHAT(eventType = "addToCart") WHEN(< 8 days before now)]))` |
| 24 時間の時間枠内での頻度イベント | 24 時間の時間枠内に特定の回数だけ発生するイベントを参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを&#x200B;**少なくとも** 5 回訪問した人物。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] )` |
| 24 時間の時間枠内でのプロファイル属性を持つ頻度イベント | 1 つ以上のプロファイル属性と、24 時間の時間枠内に一定の回数だけ発生するイベントを参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを&#x200B;**少なくとも** 5 回訪問した米国在住の人物。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] )` |
| 24 時間の時間枠内でのプロファイルを持つ頻度イベントの否定 | 1 つ以上のプロファイル属性と、24 時間の時間枠内に特定の回数だけ発生するイベントの否定を参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを 6 回&#x200B;**以上**&#x200B;訪問していない人物。 | `not(chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] ))` |
| 24 時間のタイムプロファイル以内での複数の受信ヒット | 24 時間の時間枠内に発生する複数のイベントを参照する任意のセグメント定義。 | ホームページを訪問した人、**または**&#x200B;過去 24 時間以内にチェックアウトページを訪問した人。 | `chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 24 hours before now)]) and chain(xEvent, timestamp, [X: WHAT(eventType = "checkoutPageView") WHEN(< 24 hours before now)])` |
| 24 時間の時間枠内にプロファイルを持つ複数のイベント | 1 つ以上のプロファイル属性と、24 時間以内に発生する複数のイベントを参照するセグメント定義。 | ホームページ&#x200B;**および**&#x200B;チェックアウトページにアクセスした、米国在住の人。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 24 hours before now)]) and chain(xEvent, timestamp, [X: WHAT(eventType = "checkoutPageView") WHEN(< 24 hours before now)])` |
| セグメントのセグメント | 1 つ以上のバッチセグメントまたはストリーミングセグメントを含むセグメント定義。 | 米国在住で、「既存のセグメント」のセグメントに属しているユーザー。 | `homeAddress.countryCode = "US" and inSegment("existing segment")` |
| マップを参照するクエリ | プロパティのマップを参照するセグメント定義。 | 外部セグメントデータに基づいて買い物かごに追加したユーザー。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart") WHERE(externalSegmentMapProperty.values().exists(stringProperty="active"))])` |

また、セグメントが、エッジ上でアクティブな結合ポリシーに結び付けられている&#x200B;**必要があります**。 結合ポリシーの詳細については、[結合ポリシーガイド](../../profile/api/merge-policies.md)を参照してください。

セグメント定義は次のようになります。 **not** は、次のシナリオでエッジセグメント化に対して有効になっています。

- セグメント定義には、単一のイベントと `inSegment` イベント。
   - ただし、セグメントが `inSegment` イベントはプロファイルのみ、セグメント定義 **遺言** をエッジセグメント化に対して有効にする。

## エッジセグメント化で有効なすべてのセグメントの取得

`/segment/definitions` エンドポイントに対して GET リクエストを行うことで、IMS 組織内でエッジセグメント化が有効になっているすべてのセグメントのリストを取得できます。 

**API 形式**

エッジセグメント化が有効になっているセグメントを取得するには、リクエストパスにクエリパラメーター `evaluationInfo.synchronous.enabled=true` を含める必要があります。

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

リクエストが成功すると、エッジセグメント化が有効になっている、IMS 組織内のセグメントの配列が応答として返されます。返されるセグメント定義について詳しくは、[セグメント定義エンドポイントガイド](./segment-definitions.md)を参照してください。

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

エッジセグメント化が有効なセグメントを作成するには、[上記のエッジセグメント化クエリタイプ](#query-types)のいずれかと一致する `/segment/definitions` エンドポイントに対して POST リクエストを行います。

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

>[!NOTE]
>
>次の例は、セグメントを作成するための標準的なリクエストです。 セグメント定義の作成について詳しくは、[セグメントの作成](../tutorials/create-a-segment.md)に関するチュートリアルを参照してください。

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

リクエストが成功すると、エッジセグメント化が有効な新規作成のセグメント定義の詳細が応答として返されます。

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

これで、エッジセグメント化が有効なセグメントの作成方法がわかったので、それらを使用して、同じページと次のページのパーソナライゼーションユースケースを有効にすることができます。

Adobe Experience Platform ユーザーインターフェイスを使用して同様のアクションを実行しセグメントを操作する方法については、[セグメントビルダーユーザーガイド](../ui/segment-builder.md)を参照してください。

## 付録

次の節では、エッジセグメント化に関するよくある質問を一覧表示します。

### Edge Network 上でセグメントが使用可能になるまで、どれくらいかかりますか？

Edge Network 上でセグメントが使用可能になるまで、最大 1 時間かかります。